<script>
import axios from "axios";
export default {
  data: function () {
    return {
      products: [],
    };
  },
  created: function () {
    this.indexProducts();
  },
  methods: {
    indexProducts: function () {
      axios.get("/products").then((response) => {
        console.log("products index", response);
        this.products = response.data;
      });
    },
  },
};
</script>

<template>
  <div class="products-index">
    <h1>All Products</h1>
    <div v-for="product in products" v-bind:key="product.id">
      <h2>{{ product.name }}</h2>
      <img v-bind:src="product.image" v-bind:alt="product.name" />
      <p>Description: {{ product.description }}</p>
      <p>Price: {{ product.price }}</p>
      <p>Supplier_id: {{ product.supplier_id }}</p>
      <p>Quantity: {{ product.quantity }}</p>
    </div>
  </div>
</template>
