<template>
  <ProductForm/>
</template>


<script>
import ProductForm from './components/ProductForm.vue'

export default {
  name: 'App',
  components: {
    ProductForm
  }
}
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin: 60px;
}
</style>
