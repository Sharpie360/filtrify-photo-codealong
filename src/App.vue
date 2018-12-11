<template>
  <div id="app">
    <nav class="navbar">
      <p class="navbar-brand mb-0 py-0">Filtrify Photo</p>
    </nav>
    
    <div class="layout-grid">
      <div class="control-panel">
        <cp--image-loader></cp--image-loader>
        <cp--filter-panel></cp--filter-panel>
      </div>
      
      <ic--display-image
        :image="image">
      </ic--display-image>
      
    <div class="canvas--wrapper" v-show="false">
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
import eventBus from './eventBus.js'
import ImageLoader from './components/control-panel/ImageLoader'
import FilterPanel from './components/control-panel/FilterPanel'
import DisplayImage from './components/image-container/DisplayImage'

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
      },
      
    }
  },
  components: {
    'cp--image-loader': ImageLoader,
    'cp--filter-panel': FilterPanel,
    'ic--display-image': DisplayImage
  },
  created() {
    eventBus.$on('loadImage', newImage => {
      this.image.source = newImage.source
      this.getImageSizePX()
    })
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
      // const fullQuality = canvas.toDataURL('image/png', .5)
      const medQuality = canvas.toDataURL('image/jpeg', 0.5)
      const lowQuality = canvas.toDataURL('image/jpeg', 0.1)

      downloadURI(medQuality, 'test.jpeg')
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
.flexbox {
  display: flex;
}
.flex-1 {
  flex: 1;
}
.flex-2 {
  flex: 2;
}
.flex-3 {
  flex: 3;
}
.flexbox-space-between {
  display: flex;
  flex: 1;
  justify-content: space-between;
}
.flexbox-space-evenly {
  display: flex;
  flex: 1;
  justify-content: space-evenly;
}
.flexbox-space-around {
  display: flex;
  flex: 1;
  justify-content: space-around;
}
.flex-align-center {
  align-items: center;
}
.flex-justify-center {
  justify-content: center
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
