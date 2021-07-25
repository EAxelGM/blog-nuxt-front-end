<template>
  <div>
    <v-card class="my-5">
      <v-toolbar class="primary" dark flat >
        <v-toolbar-title>{{post.title}}</v-toolbar-title>
      </v-toolbar>
      <v-card-subtitle>
        Realizado por: {{post.user.name}} {{post.user.lastname}}
      </v-card-subtitle>
      <v-container>{{post.subtitle}}</v-container>
      <v-card-subtitle v-if="post.category">
        Categoria: <a :href="`category/${post.category.slug}`">{{post.category.name}}</a>
      </v-card-subtitle>
      <v-row>
        <v-col align="center">
          <v-card max-width="450">
            <v-img
              :src="post.image"
            />
          </v-card>
        </v-col>
      </v-row>
      <v-card-subtitle>
        Etiquetas:
        <label v-for="(tag, index) in post.tags_ids" :key=index >
          <a :href="`tag/${tag.slug}`" v-if="tag">{{tag.name}} </a>
        </label>
      </v-card-subtitle>
      <v-card-actions v-if="$auth.user._id == post.user_id">
        <v-row>
          <v-col>
            <v-btn>
              Modificar
            </v-btn>
          </v-col>
          <v-col></v-col>
        </v-row>
      </v-card-actions>
    </v-card>
  </div>
</template>

<script>
export default {
  props:['post'],
  data(){
    return{

    }
  },
  mounted(){

  },
  methods:{

  }
}
</script>

<style>

</style>