<template>
  <div class="container">
    <h4>Total de Productos: {{ products.length }}</h4>
    <div class="row">
      <div class="col-md-12">
        <div v-for="product in products">
          <div>
            {{ product.title }}
            <button                
              data-toggle="collapse"
              v-bind:href="'#'+product.code.web"
              role="button"
              aria-expanded="false"
              v-bind:aria-controls="product.code.web"
              class="btn btn-success btn-sm"
              style="float:right"
            >anuncios de meli +</button>
          </div>
          <hr />

          <div class="collapse" v-bind:id="product.code.web">
            <div class="card card-body">
              <div v-for="meli in product.mercadolibre">
                <b>Tipo de publicación:</b>
                {{ meli.type }}
                <br />
                <b>Código MercadoLibre:</b>
                {{ meli.code }}
                <br />
                <b>Precio MercadoLibre:</b>
                {{ meli.price }}
                <br />
                <b>Porcentaje de recargo aplicado:</b>
                {{ meli.percent }}
                <hr />
              </div>
            </div>
          </div>
        </div>
      </div>
    </div>
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
    const products = await axios.get("https://meli-nodejs-morano.herokuapp.com/product");
    this.products = products.data;
  },
  methods: {
    viewDiv: function(id) {
      var element = document.getElementById(id);
      var element = document.getElementById(id);
      element.classList.add("d-block");
    }
  }
};
</script> 