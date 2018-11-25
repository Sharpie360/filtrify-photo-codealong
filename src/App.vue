<template>
  <div id="app">
    <nav class="navbar">
      <p class="navbar-brand mb-0 py-0">Filtrify Photo</p>
    </nav>
    
    <div class="layout-grid">
      <div class="control-panel">

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
              v-model="image.source"
              @input="getImageSizePX">
          </div>
        </div>

        <div class="filter--panel px-3 pt-2">
          <h4>Filters</h4>
          <div class="form-group" 
            v-for="(filter, i) in filters" 
            :key="i">
            <label 
              class="mb-0"
              :for="filter.name">{{ filter.name }}
            </label>
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
          <div class="action-buttons flexbox-space-between">
            <button 
              class="btn btn-primary py-1"
              @click="saveImage">
              Save Image
            </button>
            <button class="btn btn-outline-danger py-1">
              Reset Filters
            </button>
          </div>
        </div>
        
      </div>
      
      <section class="display--container my-4">
        <img 
          id="image"
          class="display--image card" 
          crossOrigin="Anonymous"
          v-show="image.source"
          :class="{ 'image-loaded': image.source }"
          :src="image.source" 
          alt="">
      </section>
    <div class="canvas--wrapper" v-show="true">
      <canvas 
        id="canvas" 
        :width="image.size.width"
        :height="image.size.height">
      </canvas>
    </div>
    </div>

      
  </div>
</template>

<script>

function downloadURI(uri, name) {
  let link = document.createElement("a");
  link.download = name;
  link.href = uri;
  document.body.appendChild(link);
  link.click();
  document.body.removeChild(link);
  link.remove();
}

export default {
  name: 'app',
  data () {
    return { 
      image: {
        source: '',
        size: {
          width: 0,
          height: 0
        },
        downloadQuality: 'fullQuality'
      },
      filters: [
        {
          name: 'blur',
          min: 0,
          max: 5,
          current: 0,
          suffix: 'px'
        },
        {
          name: 'brightness',
          min: 50,
          max: 150,
          current: 100,
          suffix: '%'
        },
        {
          name: 'contrast',
          min: 50,
          max: 250,
          current: 100,
          suffix: '%'
        },
        {
          name: 'hue-rotate',
          min: 0,
          max: 360,
          current: 0,
          suffix: 'deg'
        },
        {
          name: 'invert',
          min: 0,
          max: 100,
          current: 0,
          suffix: '%'
        },
        {
          name: 'saturate',
          min: 0,
          max: 300,
          current: 100,
          suffix: '%'
        },
        {
          name: 'sepia',
          min: 0,
          max: 100,
          current: 0,
          suffix: '%'
        }
      ]
    }
  },
  created() {
   
  },
  methods: {
    getImageSizePX () {
      const image = document.getElementById('image')
      const imageComputedStyles = window.getComputedStyle(image)
      // allow cpu to compute property to return px value
      setTimeout(() => {
        this.image.size.width = imageComputedStyles.width
        this.image.size.height = imageComputedStyles.height
      }, 100)
            
    },
    // event method to update filter css variables
    updateFilterValue(filter) {
      console.log(`--${filter.name}`, `${filter.current}${filter.suffix}`)
      document.documentElement.style.setProperty(`--${filter.name}`, `${filter.current}${filter.suffix}`)
    },
    saveImage(){
      const filters = this.filters
      let filterString = '';
      filters.forEach(filter => {
        filterString += `${filter.name}(${filter.current}${filter.suffix}) `
      })
      console.log(filterString)
        
      
      const image = document.getElementById('image')
      const canvas = document.getElementById('canvas')
      const ctx = canvas.getContext('2d')
      ctx.filter = filterString
      ctx.drawImage(image, 0, 0, canvas.width, canvas.height);
      console.log(ctx)
      const imageURL = canvas.toDataURL();
      const fullQuality = canvas.toDataURL('image/png', .5)
      const medQuality = canvas.toDataURL('image/jpeg', 0.5)
      const lowQuality = canvas.toDataURL('image/jpeg', 0.1)

      downloadURI(fullQuality, 'test.jpeg')
    }
  },


};
</script>

<style>
@import './assets/css/bootstrapv3.css';

/*
  root level css vars, targeted with javascript
*/
:root {
  --blur: 0px;
  --brightness: 100%;
  --contrast: 100%;
  --hue-rotate: 0deg;
  --invert: 0%;
  --saturate: 100%;
  --sepia: 0%;
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

/* 
  adjust height to account for navbar height CALC 
  18rem for panel, auto for auto size container, for collapsing the panel
*/
.layout-grid {
  display: grid;
  grid-template-columns: 18rem auto;
  height: calc(100% - 3rem);
}

.control-panel {
  border-right: 1px solid #000;
}

.filter--panel {
  border-top: 1px solid #000;
}

/* 
  flexbox utility class used globally
*/
.flexbox-space-between {
  display: flex;
  flex: 1;
  justify-content: space-between
}

/*
  flexbox on display container to center image vertically and horizontally
*/
.display--container {
  display: flex;
  flex: 1;
  justify-content: center;
  align-items: center;
}


/*
  filter list
  max width of image relative to container
  use set width inside set range
*/
.display--image {
  filter: 
    blur(var(--blur))
    brightness(var(--brightness))
    contrast(var(--contrast))
    hue-rotate(var(--hue-rotate))
    invert(var(--invert))
    saturate(var(--saturate))
    sepia(var(--sepia));
    
  max-width: 90%;
}

.display--image.image-loaded {
  border:1px solid #000;
  padding: 1rem;
}



</style>
