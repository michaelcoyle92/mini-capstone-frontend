<script>
import axios from "axios";
export default {
  data: function () {
    return {
      product: {},
    };
  },
  created: function () {
    axios.get("/products/" + this.$route.params.id).then((response) => {
      console.log("products show", response);
      this.product = response.data;
    });
  },
  methods: {
    destroyPhoto: function (product) {
      axios.delete("/products/" + product.id).then((response) => {
        console.log("products destroy", response);
        this.$router.push("/products");
      });
    },
  },
};
</script>

<template>
  <div class="products-show">
    <h2>{{ product.name }}</h2>
    <img v-bind:src="product.url" v-bind:alt="product.name" />
    <p>Description: {{ product.description }}</p>
    <p>Price: {{ product.price }}</p>
    <p>Supplier_id: {{ product.supplier_id }}</p>
    <p>Quantity: {{ product.quantity }}</p>
    <p></p>
    <router-link v-bind:to="`/products/${product.id}/edit`">Edit product</router-link>
    <button v-on:click="destroyProduct(product)">Destroy product</button>
    <router-link to="/products">Back to all products</router-link>
  </div>
</template>
