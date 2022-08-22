<script>
import axios from "axios";
export default {
  data: function () {
    return {
      message: "Welcome to Vue.js!",
      products: [],
      newProduct: {}
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
    },
    createProduct: function () {
      // var params = {
      //   name: "Table",
      //   price: 200,
      //   description: "Put anything you want on this bad boy. You can even eat on it.",
      //   image_url: "https://alacraterentals.com/wp-content/uploads/2018/02/ALaCrate-Rentals-Table-Harvest-ThreeQuarterView-Wood-Table-Rental-Weddings-Events-MadeInWisconsin-WEB-600x600-ALM2-600x600.jpg"
      // }
      axios.post("http://localhost:3000/products.json", this.newProduct).then(response => {
        console.log(response.data)
        this.products.push(response.data);
      })
    }
  }
};
</script>

<template>
  <div class="home">
    <p> Name: <input type="text" v-model="newProduct.name" /> </p>
    <p> Price: <input type="text" v-model="newProduct.price" /> </p>
    <p> Description: <input type="text" v-model="newProduct.description" /> </p>
    <p> Image URL: <input type="text" v-model="newProduct.image_url" /> </p>
    <button v-on:click="createProduct">Create Product</button>
    <div v-for="product in products">
      <p> {{ product.name }} </p>
      <p> ${{ product.price }} </p>
      <p> {{ product.description }} </p>
      <!-- <img v-bind:src="product.image_url" /> -->
      <br />
      <br />
    </div>
  </div>
</template>

<style>
</style>