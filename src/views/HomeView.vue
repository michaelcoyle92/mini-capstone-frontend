<script>
import axios from "axios";
export default {
  data: function () {
    return {
      products: [],
      newProductParams: {},
      editProductParams: {},
      currentProduct: {},
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
    createPhoto: function () {
      axios
        .post("/products", this.newProductParams)
        .then((response) => {
          console.log("products create", response);
          this.products.push(response.data);
          this.newProductParams = {};
        })
        .catch((error) => {
          console.log("products create error", error.response);
        });
    },
    showProduct: function (product) {
      this.currentproduct = product;
      this.editProductParams = product;
      document.querySelector("#product-details").showModal();
    },
    updateProduct: function (product) {
      axios
        .patch("/products/" + product.id, this.editProductParams)
        .then((response) => {
          console.log("products update", response);
          this.currentProduct = {};
        })
        .catch((error) => {
          console.log("products update error", error.response);
        });
    },
    destroyProduct: function (product) {
      axios.delete("/products/" + product.id).then((response) => {
        console.log("products destroy", response);
        var index = this.products.indexOf(product);
        this.products.splice(index, 1);
      });
    },
  },
};
</script>

<template>
  <div class="home">
    <h1>New Product</h1>
    <div>
      Name:
      <input type="text" v-model="newProductParams.name" />
      Description:
      <input type="text" v-model="newProductParams.description" />
      Price:
      <input type="text" v-model="newProductParams.price" />
      Supplier_id:
      <input type="text" v-model="newProductParams.supplier_id" />
      Quantity:
      <input type="text" v-model="newProductParams.quantity" />
      <button v-on:click="createProduct()">Create Product</button>
    </div>
    <h1>All Products</h1>
    <div v-for="product in products" v-bind:key="product.id">
      <h2>{{ product.name }}</h2>
      <img v-bind:src="product.image" v-bind:alt="product.name" />
      <p>Description: {{ Product.description }}</p>
      <p>Price: {{ Product.price }}</p>
      <p>Supplier_id: {{ Product.supplier_id }}</p>
      <p>Quantity: {{ Product.quantity }}</p>
      <button v-on:click="showProduct(product)">More info</button>
    </div>
    <dialog id="product-details">
      <form method="dialog">
        <h1>Product info</h1>
        <p>
          Name:
          <input type="text" v-model="editProductParams.name" />
        </p>
        <p>
          Description:
          <input type="text" v-model="editProductParams.description" />
        </p>
        <p>
          Price:
          <input type="text" v-model="editProductParams.price" />
        </p>
        <p>
          Suplier_id:
          <input type="text" v-model="editProductParams.supplier_id" />
        </p>
        <p>
          Quantity:
          <input type="text" v-model="editProductParams.quantity" />
        </p>
        <p>
          Image:
          <input type="text" v-model="editProductParams.image" />
        </p>
        <button v-on:click="updateProduct(currentProduct)">Update</button>
        <button v-on:click="destroyProduct(currentProduct)">Destroy Product</button>
        <button>Close</button>
      </form>
    </dialog>
  </div>
</template>

<style></style>
