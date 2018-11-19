<template>
  <div id="app">
    <h2>Filtrify Photo</h2>
    <div class="photo--container">
      <img 
        class="photo--image" 
        :src="imageSource" 
        alt="">
    </div>
    <div class="image-loader container">
      <div class="form-group">
        <label for="custom-image-input">Custom Image URL</label>
        <input type="text" class="form-control" id="custom-image-input" v-model="imageSource">
      </div>
    </div>
    <div class="filter-panel">

    </div>
  </div>
</template>

<script>

export default {
  name: 'app',
  data () {
    return { 
      imageSource: '',
      filters: {
        blur: 0,          // px
        brightness: 0,    // %
        contrast: 0,      // %
        grayscale: 0,     // %
        hueRotate: 0,     // deg
        invert: 0,        // %
        opacity: 0,       // %
        saturate: 0,      // %
        sepia: 0,         // %
      }
    }
  },

};
</script>

<style>
@import './assets/css/bootstrapv3.css';

#app {
  font-family: 'Avenir', Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}

.photo--container {
  min-height: 80vh;
  display: flex;
  flex: 1;
  justify-content: center;
  align-items: center;
}

.photo--image {
  max-height: 80vh;
}
</style>
