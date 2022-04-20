<script>
import axios from "axios";
export default {
  data: function () {
    return {
      product: {},
      editProductParams: {},
      errors: [],
    };
  },
  created: function () {
    axios.get("/products/" + this.$route.params.id).then((response) => {
      console.log("products show", response);
      this.product = response.data;
      this.editProductParams = this.product;
    });
  },
  methods: {
    updateProduct: function (product) {
      axios
        .patch("/products/" + product.id, this.editProductarams)
        .then((response) => {
          console.log("products update", response);
          this.$router.push("/products");
        })
        .catch((error) => {
          console.log("products update error", error.response);
          this.errors = error.response.data.errors;
        });
    },
  },
};
</script>

<template>
  <div class="products-edit">
    <h1>Edit Product</h1>
    <form v-on:submit.prevent="updateProduct(product)">
      <ul>
        <li v-for="error in errors" v-bind:key="error">{{ error }}</li>
      </ul>
      Name:
      <input type="text" v-model="editProductParams.name" />
      Description:
      <input type="text" v-model="editProductParams.description" />
      Price:
      <input type="text" v-model="editProductParams.price" />
      Created_at:
      <input type="text" v-model="editProductParams.created_at" />
      Updated_at:
      <input type="text" v-model="editProductParams.updated_at" />
      Supplier_id:
      <input type="text" v-model="editProductParams.supplier_id" />
      Quantity:
      <input type="text" v-model="editProductParams.quantity" />
      <input type="submit" value="Update" />
    </form>
  </div>
</template>
