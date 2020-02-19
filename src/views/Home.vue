<template>
  <div class="container">
    <div class="home">
      <h1>{{ message }}</h1>
      <button v-on:click="createProduct()">Create a NEW product!!!!!!!!!!</button>
      <br>
      <br>
      <button v-on:click="gtfo()">Ready for more products? Click here!</button>
      <div v-for="product in products">
        <h2> {{ product.name }} </h2>
        <p>Description: {{ product.description }} </p>
        <p>Price (tax included): {{ product.formatted.total }} </p>
        <img v-bind:src="product.image_url">
      </div>
    </div>
  </div>
</template>

<style>
</style>

<script>
let axios = require("axios");

export default {
  data: function() {
    return {
      message: "SWEET SWEET PRODUCTS BABY!",
      products: [],
    };
  },
  created: function() {
    axios.get("/api/products").then(response => {
      this.products = response.data;
      console.log(response.data)
    });
  },
  methods: {
    gtfo: function(){
      window.location.href="https://media0.giphy.com/media/6g6fIvdYMZPws/giphy.gif"
    },
    createProduct: function(){
      console.log("Product created!")

      let params = {
        name: "potatos",
        description: "boil em, mash em, put em in a stew",
        image_url: "https://media0.giphy.com/media/6g6fIvdYMZPws/giphy.gif",
        price: "666.69",
      }
      axios.post("/api/products", params).then(response => {
        this.products.push(respons.data);
      });
    },
  }
};
</script>