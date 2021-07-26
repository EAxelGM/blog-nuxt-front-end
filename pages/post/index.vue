<template>
  <div>
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

    <v-row v-if="!loading">
      <v-col v-for="(post, index) in posts" :key="index" cols="12" md="4">
        <ViewPost :post="post" />
      </v-col>
    </v-row>

    <v-row v-else>
      <v-col v-for="(number, index) in 9" :key="index" cols="12" md="4">
        <EsqueletoLoading />
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
import EsqueletoLoading from '@/components/Posts/EsqueletoMuchosPost';
export default {
  components:{
    ViewPost,
    EsqueletoLoading
  },
  layout: 'logged',
  data(){
    return{
      pagina: 1,
      fullPaginacion: 0,
      posts: [],
      firsLoading: true,
      loading: false,
    }
  },
  mounted(){
    this.getPosts();
    this.firstLoading = false;
  },
  methods:{
    async getPosts(){
      this.loading = true;
      const url = `post?page=${this.pagina}`
      try {
        const {data} = await this.$axios.get(url);
        this.posts = data.data.data;
        this.fullPaginacion = data.data.last_page;
      } catch (error) {
        console.log({error});
      }
      this.loading = false;
    }
  }
}
</script>

<style>

</style>