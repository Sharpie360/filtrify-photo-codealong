<template>
  <div class="image--loader px-3 pt-2">
    <div class="form-group">
      <label 
        for="custom-image-input"
        class="mb-0">
        <h4>Image URL</h4>
      </label>
      <input 
        type="text" 
        class="form-control" 
        id="custom-image-input" 
        v-model="imageInput.source"
        @input="loadImage">
    </div>
  </div>
</template>

<script>
import eventBus from '../../eventBus.js'

export default {
  data () {
    return {
      imageInput: {
        source: ''
      }
    }
  },
  methods: {
    loadImage() {
      eventBus.$emit('loadImage', this.imageInput)
    }
  }
}
</script>

<style>

</style>
