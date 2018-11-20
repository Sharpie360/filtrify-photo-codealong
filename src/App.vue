<template>
  <div id="app">
    <nav class="navbar">
      <p class="navbar-brand mb-0 py-0">Filtrify Photo</p>
    </nav>
    
    <div class="layout-grid">
      <div class="control-panel">

        <div class="image--loader px-3 pt-2">
          <div class="form-group">
            <label for="custom-image-input">
              <h4>Image URL</h4>
            </label>
            <input type="text" class="form-control" id="custom-image-input" v-model="image.source">
          </div>
        </div>

        <div class="filter--panel px-3 pt-2">
          <h4>Filters</h4>
          <div class="form-group" v-for="(filter, i) in filters" :key="i">
            <label :for="filter.name">{{ filter.name }}</label>
            <input 
              class="form-control" 
              type="range" 
              v-model="filter.current"
              @mousemove="updateFilterValue(filter)"
              @change="updateFilterValue(filter)"
              :id="filter.name"
              :min="filter.min"
              :max="filter.max" 
              :aria-valuemin="filter.min"
              :aria-valuemax="filter.max"
              :aria-valuenow="filter.current">
          </div>
        </div>
        
      </div>
      
      <section class="display--container my-4">
        <img 
          class="display--image card" 
          :src="image.source" 
          alt="">
      </section>
      
    </div>
  </div>
</template>

<script>

const displayImageTag = document.querySelector('.display--image');

export default {
  name: 'app',
  data () {
    return { 
      image: {
        source: '',
        maxWidth: '90%',
        userSetWidth: '',
      },

      filters: {
        blur: {
          name: 'blur',
          min: 0,
          max: 40,
          current: 0,
          suffix: 'px'
        },
        brightness: {
          name: 'brightness',
          min: 25,
          max: 150,
          current: 100,
          suffix: '%'
        }
        // brightness: 0,    // %
        // contrast: 0,      // %
        // grayscale: 0,     // %
        // hueRotate: 0,     // deg
        // invert: 0,        // %
        // opacity: 0,       // %
        // saturate: 0,      // %
        // sepia: 0,         // %
      }
    }
  },
  methods: {
    updateFilterValue(filter, ) {

      console.log(`--${filter.name}`, `${filter.current}${filter.suffix}`)
      document.documentElement.style.setProperty(`--${filter.name}`, `${filter.current}${filter.suffix}`)
    }
  },


};
</script>

<style>
@import './assets/css/bootstrapv3.css';

:root {
  --blur: 0px;
  --brightness: 0%;

}

/* consistant full bg color regardless of size */ 
body {
  background-color: rgb(246, 246, 246);
}

#app {
  font-family: 'Avenir', Helvetica, Arial, sans-serif;
  color: #0f0f0f;
  height: 100vh;
  position: relative;
}

/* set default height to 3rem of 12px */
.navbar {
  background-color: rgb(15, 15, 15);
  color: rgb(246, 246, 246);
  height: 3rem;
}

/* adjust height to account for navbar height CALC */
.layout-grid {
  display: grid;
  grid-template-columns: 1fr 4fr;
  height: calc(100% - 3rem);
}

.control-panel {
  border-right: 1px solid #000;
}

.filter--panel {
  border-top: 1px solid #000;
}

.display--container {
  display: flex;
  flex: 1;
  justify-content: center;
  align-items: center;
}

.display--image {

  filter: blur(var(--blur));
  filter: brightness(var(--brightness));

  max-width: 90%;
  border: 1px solid #212121;
}



</style>
