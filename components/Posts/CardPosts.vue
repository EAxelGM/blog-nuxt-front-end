<template>
  <div>
    <v-card class="my-5" max-width="450" :color="post.status=='DRAFT' ? '#FFD63399' : ''" >
      <v-row>
        <v-col>
          <v-card-title>{{post.title}}</v-card-title>
          <v-card-subtitle>Realizado por: {{post.user.name}} {{post.user.lastname}}</v-card-subtitle>
        </v-col>
        <v-col>
          <v-img
            :src="post.image"
            height="250"
          />
        </v-col>
      </v-row>
      <v-card-actions>
        <v-btn block color="primary" :to="`post/${post.slug}`">
          Ver Post
        </v-btn>
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