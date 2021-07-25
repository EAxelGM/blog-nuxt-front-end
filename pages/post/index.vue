<template>
  <div>
    <v-row>
      <v-col v-for="(post, index) in posts" :key="index" cols="12" md="4">
        <ViewPost :post="post" />
      </v-col>
    </v-row>

    <v-row justify="center" v-if="fullPaginacion > 0">
        <v-col cols="8">
          <v-container class="max-width" @click="getPosts()">
            <v-pagination
              v-model="pagina"
              class="my-4"
              :total-visible="6"
              circle
              :length="fullPaginacion"
            />
          </v-container>
        </v-col>
      </v-row>
  </div>
</template>

<script>
import ViewPost from '@/components/Posts/CardPosts';
export default {
  components:{
    ViewPost,
  },
  layout: 'logged',
  data(){
    return{
      pagina: 1,
      fullPaginacion: 0,
      posts: [],
    }
  },
  mounted(){
    this.getPosts();
  },
  methods:{
    async getPosts(){
      const url = `post?page=${this.pagina}`
      try {
        const {data} = await this.$axios.get(url);
        this.posts = data.data.data;
        this.fullPaginacion = data.data.last_page;
      } catch (error) {
        console.log({error});
      }
    }
  }
}
</script>

<style>

</style>