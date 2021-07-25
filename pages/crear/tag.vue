<template>
  <div>
    <v-card>
      <v-toolbar class="primary" dark flat >
        <v-toolbar-title>Crea una etiqueta!</v-toolbar-title>
      </v-toolbar>
      <v-container>
        <v-row>
          <v-col cols="12" md="8" align="justify">
            <v-label>
              Recuerda que todas las etiquetas creadas, se tiene un registro de usuario de quienes las han creados, en dado caso de crear etiquetas no apropiadas, la cuenta puede llegar a ser suspendida, porfavor cree las etiquetas de manera correcta.
            </v-label>
          </v-col>
          <v-col cols="12" md="4" align="center">
            <v-text-field 
              label="Ingresa el nombre de la etiqueta"
              v-model="name"
            />
            <v-btn color="primary" :disabled="name==''" :loading="loading" @click="crear()">
              Crear Etiqueta
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
      name: '',
      loading: false,
    }
  },
  methods:{
    async crear(){
      this.loading = true;
      const url = 'tag';
      const info = {
        name: this.name,
        user_id: this.$auth.user._id
      };
      try {
        const { data } = await this.$axios.post(url,info);
        await this.$store.dispatch('alertas/GET_DATA', {
          color : 'success',
          snackbar : true,
          data : data.message
        });
        this.name = "";
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