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
      currentProduct: {},
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
    showProduct: function (product) {
      console.log(product);
      this.currentProduct = product;
      document.querySelector("#product-details").showModal();
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
        <br />
        <button @click="showProduct(product)">click for more info</button>
      </div>
      <dialog id="product-details">
        <form method="dialog">
          <h1>Product Info</h1>
          <p>name: {{ currentProduct.name }}</p>
          <p>description: {{ currentProduct.description }}</p>
          <p>price: {{ currentProduct.price }}</p>
          <button>Close</button>
        </form>
      </dialog>
    </div>
  </div>
</template>

<style>
img {
  width: 250px;
}
</style>
