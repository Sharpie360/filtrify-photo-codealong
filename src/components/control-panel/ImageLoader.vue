<template>
  <div class="image--loader px-3 pt-2">
    <div class="upload-type--outer flexbox-space-between flex-align-center">
      <h5 class="upload-type--label mb-0">Upload Type:</h5>
      <div class="btn-group upload-type--btn-group">
        <button class="btn btn-primary py-1 ">URL</button>
        <button class="btn btn-secondary py-1">File</button>
      </div>

    </div>
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
      urlInput: true,  
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
.upload-type--label {
  display: inline-block;
}
</style>
