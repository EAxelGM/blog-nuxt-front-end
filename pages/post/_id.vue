<template>
  <div>
    <CardPost :post="post" />
  </div>
</template>

<script>
import CardPost from '@/components/Posts/CardPost';
export default {
  components:{
    CardPost,
  },
  layout: 'logged',
  data(){
    return{
      
    }
  },
  async asyncData ({ $axios, params }) {
    console.log({params});
    const { data } = await $axios.get(`post/${params.id}`)
    return { post: data.data}
  },
}
</script>

<style>

</style>