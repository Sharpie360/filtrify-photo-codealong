<template>
  <div id="app">
    <nav class="navbar">
      <p class="navbar-brand mb-0 py-0">Filtrify Photo</p>
    </nav>
    
    <div class="layout-grid">
      <div class="control-panel">
        <div class="image-loader container">
          <div class="form-group">
            <label for="custom-image-input">Image URL</label>
            <input type="text" class="form-control" id="custom-image-input" v-model="image.source">
          </div>
        </div>

        <div class="filter-panel">

        </div>
        
      </div>
      
      <section class="photo--container my-4">
        <img 
          class="photo--image card" 
          :src="image.source" 
          alt="">
      </section>
      
    </div>
  </div>
</template>

<script>

export default {
  name: 'app',
  data () {
    return { 
      image: {
        source: '',
        maxWidth: '90%',
        userSetWidth: ''
      },

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
  color: #0f0f0f;
  height: 100vh;
  background-color: rgb(246, 246, 246);
  position: relative;
}

.navbar {
  background-color: rgb(15, 15, 15);
  color: rgb(246, 246, 246);
  height: 3rem;
}

.layout-grid {
  display: grid;
  grid-template-columns: 1fr 4fr;
  height: calc(100% - 3rem);
}

.control-panel {
  
}

.filter-panel {
  border-top: 1px solid #000;
}

.photo--container {
  display: flex;
  flex: 1;
  justify-content: center;
  align-items: center;
}

.photo--image {
  max-width: 90%;
  border: 1px solid #212121;
}



</style>
