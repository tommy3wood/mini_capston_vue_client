<template>
  <div class="container">
    <div class="home">
      <h1>{{ message }}</h1>

      <div class="new-form">
        Name: <input type="text" v-model="newProductName"> 
        Description: <input type="text" v-model="newProductDescription"> 
        Image: <input type="text" v-model="newProductImageUrl"> 
        Price: <input type="text" v-model="newProductPrice"> 
        <button v-on:click="createProduct()">Create a NEW product!!!!!!!!!!</button> 
        <br>
        <br>
      </div>
      <br>
      
      <div v-for="product in products">
      
        <img v-on:click="showProduct(product)" v-bind:src="product.image_url">
        
        <h5>{{ product.name }}</h5>
        <div v-if="product === currentProduct">
          <p >Description: {{ product.description }} </p>
          <p >Price (tax included): {{ product.formatted.total }} </p>
          <br>
          <div class="edit-form">

            <h4>Edit Product</h4>

            <div>
              Name: <input type="text" v-model="product.name">
            </div>

            <div>
              Description: <input type="text" v-model="product.description">
            </div>

            <div>
              Image: <input type="text" v-model="product.image_url">
            </div>

            <div>
              Total: <input type="text" v-model="product.total">
            </div>

            <button v-on:click="updateProduct(product)">Update</button>
          </div>
          <br>
          <div class="destroy-action">
            <button v-on:click="destroyProduct(product)">DESTROY IT!</button>
          </div>

        </div>

      </div>
      
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
      currentProduct: {},
      message: "SWEET SWEET PRODUCTS BABY!",
      products: [],
      newProductName: "",
      newProductDescription: "",
      newProductImageUrl: "",
      newProductPrice: "",
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

      let params = {
        name: this.newProductName,
        description: this.newProductDescription,
        image_url: this.newProductImageUrl,
        price: this.newProductPrice,
      }

      axios.post("/api/products", params)
      .then(response => {
        this.products.push(respons.data);
      }).catch(error => {
        console.log(error.response);
      });
      // this.newProductName = "",
      // this.newProductDescription = "",
      // this.newProductImageUrl = "",
      // this.newProductPrice = "",
    },
    showProduct: function(inputProduct) {
      if (this.currentProduct !== inputProduct) {
        this.currentProduct = inputProduct;
      } else {
        this.currentProduct = {};
      }
    },

    updateProduct: function(inputProduct) {
      var clientParams = {
        name: inputProduct.name,
        description: inputProduct.description,
        image_url: inputProduct.image_url,
        price: inputProduct.price
      };
      axios
        .patch("/api/products/" + inputProduct.id, clientParams)
        .then(response => {
          console.log("success", response.data);
        }).catch(error => {
        console.log(error.response.data);
        });
    },
    destroyProduct: function(inputProduct) {
      axios
        .delete("/api/products/" + inputProduct.id)
        .then(response => {
          console.log("success", response.data);
          var index = this.products.indexOf(inputProduct);
          this.products.splice(index, 1);
      });
    }
  }
};
</script>