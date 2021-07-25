<template>
  <div>
    <v-card>
      <v-toolbar class="primary" dark flat >
        <v-toolbar-title>¡Crea un post ahora!</v-toolbar-title>
      </v-toolbar>
      <v-container>
        <v-row>
          <v-col>
            <v-text-field 
              label="Ingresa un nombre a tu post"
              v-model="title"
            />
          </v-col>
        </v-row>
        <v-row>
          <v-col>
            <v-textarea
              filled
              label="Ingresa un texto para tu post"
              value=""
              v-model="subtitle"
            />
          </v-col>
        </v-row>
        <v-row>
          <v-col>
            <v-label>
              Selecciona el estado de este post
            </v-label>
            <v-radio-group v-model="status" row>
              <v-radio
                value="PUBLISHED"
                label="Publico"
              />
              <v-radio
                value="DRAFT"
                label="Borrador"
              />
            </v-radio-group>
          </v-col>
          <v-col align="center" cols="12" md="4">
            <v-row>
              <v-col>
                <v-file-input
                  label="Imagen del desarrollo *" 
                  append-icon="mdi-file-image" 
                  v-model="file" 
                  accept="image/*" 
                  @change="validarImagen()"
                  chips
                />
              </v-col>
            </v-row>
            <v-row>
              <v-col>
                <v-img
                  :src="file ? fileLocal : 'http://localhost:8000/default/image-not-found.png'"
                  max-height="125"
                  contain
                  class="grey darken-4"
                />
              </v-col>
            </v-row>
          </v-col>
          <v-col>
            <v-autocomplete 
              :items="categories"
              item-text="name"
              item-value="_id"
              label="Selecciona una categoria"
              v-model="category_id"
            />
            <a href="category">Crea una nueva categoria</a>
          </v-col>
        </v-row>
        <v-row>
          <v-col>
            <v-autocomplete 
              :items="tags"
              item-text="name"
              multiple
              chips
              item-value="_id"
              label="Seleccione las etiquetas adecuadas para su post"
              v-model="tag_ids"
            />
            <a href="tag">Crea una nueva etiqueta</a>
          </v-col>
        </v-row>
        <v-row>
          <v-col>
            <v-btn block color="primary" @click="create()" :loading="loading" :disabled="title=='' || subtitle=='' || file==null || category_id=='' || status=='' || tag_ids.length==0">
              Crear Post
            </v-btn>
          </v-col>
        </v-row>
      </v-container>
    </v-card>
  </div>
</template>

<script>
export default {
  layout: 'logged',
  data(){
    return{
      title: '',
      subtitle: '',
      file: null,
      fileLocal: '',
      category_id: '',
      tag_ids: [],
      categories: [],
      tags: [],
      loading: false,
    }
  },
  mounted(){
    this.getCategorias();
    this.getEtiquetas();
  },
  methods:{
    validarImagen(){
      if(this.file != null){
        const file = this.file;
        if(file.size > 10500000){
          alert('No se aceptan imagenes con mas de 10 MB.');
          this.file = null; 
          return
        }
        var img = new Image();
        img.src = URL.createObjectURL(file, this.fileLocal = file.name);
        this.fileLocal = img.src;
      }else{
        this.fileLocal = '';
      }
    },

    async getCategorias(){
      const url = 'category';
      try {
        const {data} = await this.$axios.get(url);
        this.categories = data.data;
      } catch (error) {
        await this.$store.dispatch('alertas/GET_DATA', {
          color : 'error',
          snackbar : true,
          data : error.response.data.message
        });
      }
    },
    
    async getEtiquetas(){
      const url = 'tag';
      try {
        const {data} = await this.$axios.get(url);
        this.tags = data.data;
      } catch (error) {
        await this.$store.dispatch('alertas/GET_DATA', {
          color : 'error',
          snackbar : true,
          data : error.response.data.message
        });
      }
    },

    async create(){

      this.loading = true;
      const url = 'post';
      try {
        let formData = new FormData();
        let config = { headers: { 'Content-Type': 'multipart/form-data' } };
        formData.append('user_id', this.$auth.user._id);
        formData.append('image', this.file);
        formData.append('title', this.title);
        formData.append('subtitle', this.subtitle);
        formData.append('status', this.status);
        formData.append('category_id', this.category_id);
        formData.append('tags_ids', JSON.stringify(this.tag_ids));
        const {data} = await this.$axios.post(url,formData,config)
        await this.$store.dispatch('alertas/GET_DATA', {
          color : 'success',
          snackbar : true,
          data : data.message
        });
        this.title = '';
        this.subtitle = '';
        this.file = null;
        this.status = '';
        this.category_id = '';
        this.tag_ids = [];
        this.validarImagen();
      } catch (error) {
        await this.$store.dispatch('alertas/GET_DATA', {
          color : 'error',
          snackbar : true,
          data : error.response.data.message
        });
      }
      this.loading = false;
    }
  }
}
</script>

<style>

</style>