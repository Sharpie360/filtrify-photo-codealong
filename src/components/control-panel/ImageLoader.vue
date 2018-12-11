<template>
  <div class="image--loader px-3 pt-2">
    <div class="upload-type--outer flexbox-space-between flex-align-center">
      <h6 class="upload-type--label mb-0">Upload Type:</h6>
      <div class="btn-group upload-type--btn-group">
        <button 
          @click="urlUploadInput = true"
          :class="urlUploadInput ? 'btn-primary' : 'btn-secondary' "
          class="btn py-1"
          >URL
        </button>
        <button 
          @click="urlUploadInput = false"
          :class="!urlUploadInput ? 'btn-primary' : 'btn-secondary'"
          class="btn py-1"
          >File
        </button>
      </div>

    </div>
    <div class="form-group">
      <label 
        for="custom-image-input-url"
        class="mb-0">
        <h4>Image URL</h4>
      </label>
      <input 
        type="text" 
        class="form-control" 
        id="custom-image-input-url" 
        v-model="imageInput.source"
        @input="loadImage">
    </div>
    <div class="form-group">
      <label 
        for="custom-image-input-file"
        class="mb-0">
        <h4 class="btn btn-primary mb-0">Upload File</h4>
      </label>
      <input 
        style="opacity: 0; height: 0; padding: 0;"
        type="file" 
        class="form-control" 
        id="custom-image-input-file" 
        @input="loadImage">
    </div>
  </div>
</template>

<script>
import eventBus from '../../eventBus.js'

export default {
  data () {
    return {
      urlUploadInput: true,  
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
.upload-type--btn-group > .btn {
  padding-left: .5rem;
  padding-right: .5rem;
  font-size: .8rem;
}
</style>
