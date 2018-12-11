<template>
  <section class="display--container my-4">
    <img 
      id="image"
      class="display--image card" 
      v-show="image.source"
      :class="{ 'image-loaded': image.source }"
      :src="image.source" 
      alt="">
  </section>
</template>

<script>
export default {
  props: ['image']
}
</script>

<style>

</style>
