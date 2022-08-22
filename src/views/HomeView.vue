<script>
import axios from "axios";
export default {
  data: function () {
    return {
      message: "Welcome to Vue.js!",
      products: []
    };
  },
  created: function () {
    this.indexProducts()
  },
  methods: {
    indexProducts: function () {
      axios.get("http://localhost:3000/products.json").then(response => {
        console.log(response.data)
        this.products = response.data
      })
    }
  }
};
</script>

<template>
  <div class="home">
    <div v-for="product in products">
      <p> {{ product.name }} </p>
      <p> ${{ product.price }} </p>
      <p> {{ product.description }} </p>
      <img v-bind:src="product.image_url" />
      <br />
      <br />
    </div>
  </div>
</template>

<style>
</style>