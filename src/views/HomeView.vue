<script>
import axios from "axios";
export default {
  data: function () {
    return {
      message: "Welcome to Vue.js!",
      products: [],
      newProduct: {},
      currentProduct: {}
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
    },
    showProduct: function (theProduct) {
      console.log(theProduct)
      this.currentProduct = theProduct
      document.querySelector('#product-details').showModal();
    },
    updateProduct: function () {
      axios.patch(`http://localhost:3000/products/${this.currentProduct.id}.json`, this.currentProduct).then(response => {
        console.log(response.data)
      })
    },
    destroyProduct: function () {
      axios.delete(`http://localhost:3000/products/${this.currentProduct.id}.json`, this.currentProduct).then(response => {
        var index = this.products.indexOf(this.currentProduct)
        this.products.splice(index, 1);
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
      <button v-on:click="showProduct(product)">More Info</button>
      <br />
      <br />
    </div>



    <dialog id="product-details">
      <form method="dialog">
        <p><b>Name:</b> {{ currentProduct.name }}</p>
        <p><b>Price:</b> {{ currentProduct.price }}</p>
        <p><b>Description:</b> {{ currentProduct.description }}</p>
        <hr />
        <hr />
        <p><b>name:</b> <input type="text" v-model="currentProduct.name" /></p>
        <p><b>Price:</b> <input type="text" v-model="currentProduct.price" /></p>
        <p><b>Description:</b> <input type="text" v-model="currentProduct.description" /></p>
        <button v-on:click="updateProduct()">Update Product</button>
        <button v-on:click="destroyProduct()">Delete Product</button>
        <button>Close</button>
      </form>
    </dialog>





  </div>
</template>

<style>
</style>