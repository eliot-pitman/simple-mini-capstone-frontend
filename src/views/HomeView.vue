<script>
import axios from "axios";
export default {
  data: function () {
    return {
      message: "this my house entry please",
      secret: "you are the chosen one",
      products: [],
      newRecipeMessage: "make a new recipe!",
      broken: "er er er idk whats going on",
      newProductParams: {},
    };
  },
  created: function () {
    this.indexProducts();
  },
  methods: {
    indexProducts: function () {
      axios.get("http://localhost:3000/products.json").then((response) => {
        this.products = response.data;
        console.log("All products", this.products);
      });
    },
    createProduct: function () {
      console.log("it worked");
      axios.post("http://localhost:3000/products.json", this.newProductParams).then((response) => {
        console.log("success", response.data);
        this.products.push(response.data);
      });
    },
  },
};
</script>

<template>
  <div class="home">
    <h1>{{ message }}</h1>
    <div>
      <h1>{{ newRecipeMessage }}</h1>
      <div>
        name:
        <input type="text" v-model="newProductParams.name" />
        <br />
        description:
        <input type="text" v-model="newProductParams.description" />
        <br />
        price:
        <input type="text" v-model="newProductParams.price" />
        <br />
        image_url:
        <input type="text" v-model="newProductParams.image_url" />
      </div>
      <button @click="createProduct">create</button>
      <h1>products:</h1>
      <div v-for="product in products" :key="product.id">
        {{ product.name }}
        <br />
        {{ "$" + product.price }}
        <br />
        <img :src="product.image_url" :alt="broken" />
      </div>
    </div>
  </div>
</template>

<style>
img {
  width: 250px;
}
</style>
