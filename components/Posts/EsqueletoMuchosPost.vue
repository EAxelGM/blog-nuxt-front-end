<template>
  <div>
    <v-skeleton-loader
      v-bind="attrs"
      type="article, image, actions"
    />
  </div>
</template>

<script>
export default {
  data(){
    return{
      attrs: {
        class: 'mb-6',
        boilerplate: true,
        elevation: 2,
      },
    }
  }
}
</script>

<style>

</style>