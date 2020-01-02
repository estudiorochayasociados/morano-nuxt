<template>
  <div class="container">
    <h1 class="title mt-20">Productos ({{ products.length }})</h1>
    <h2 class="subtitle">Listado de productos en tu desarrollo web</h2>
    <hr/>
    <button @click="updateProducts">ACTUALIZAR PRODUCTOS DESDE LA WEB</button>
    <hr/>
    <table class="table is-fullwidth mt-20">
      <thead>
        <th>Producto </th>
      </thead>
      <tbody>
        <tr v-for="product in products">
          <td>{{ product.title }}</td>
        </tr>
      </tbody>
    </table>
  </div>
</template>

<script>
import axios from "axios";
export default {
  data() {
    return {
      products: []
    };
  },
  async mounted() {
    const products = await axios.get(process.env.apiUrl + "/product", this.$cookies.get("header-token"));
    this.products = products.data;
  },
  methods: {
    viewDiv: function(id) {
      var element = document.getElementById(id);
      var element = document.getElementById(id);
      element.classList.add("d-block");
    },
    updateProducts : function() {
      await axios.get(process.env.apiUrl + "/product/update-products-with-web", this.$cookies.get("header-token"));
      const products = await axios.get(process.env.apiUrl + "/product", this.$cookies.get("header-token"));
      this.products = products.data;
    }
  }
};
</script> 