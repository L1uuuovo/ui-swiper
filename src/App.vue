<template>
  <div class="app">
    <div class="container">
      <carousel 
        :autoplay="true" 
        :duration="3000"
        :inital="0"
        :hasDot="true" 
        :hasDirector="true"
        >
        <car-item v-for="(item,index) of carData" :key="index">
          <img :src="require(`./assets/image/${item.img_name}`)" />
        </car-item>
      </carousel>
    </div>

  </div>
</template>

<script>
import carData from './data/carousel'
export default {
  name: 'App',
  setup() {
    return{
      carData
    }
  }
}
</script>

<style scoped>
  .container{
    width: 520px;
    height: 280px;
    margin: 150px auto;
  }
</style>
