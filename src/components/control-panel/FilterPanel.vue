<template>
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
</template>

<script>
export default {
  data () {
    return {
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
  methods: {
    // event method to update filter css variables
    updateFilterValue(filter) {
      console.log(`--${filter.name}`, `${filter.current}${filter.suffix}`)
      document.documentElement.style.setProperty(`--${filter.name}`, `${filter.current}${filter.suffix}`)
    },
  }
}
</script>

<style>

</style>
