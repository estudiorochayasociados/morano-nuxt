<template>
  <div class="container">
    <h2>TOTAL DE PRODUCTOS A AGREGAR: {{ products.length }}</h2>
    <br />
    <div class="row">
      <div class="col-md-3">
        Agregar características:
        <br />
        <input type="text" class="form-control" v-model="tag" />
      </div>

      <div class="col-md-3">
        Tipo de publicación:
        <br />
        <select class="form-control" v-model="type">
          <option value="gold_special">CLÁSICA (13%)</option>
          <option value="gold_pro">PREMIUM (28%)</option>
        </select>
      </div>

      <div class="col-md-3">
        Agregar porcentaje:
        <br />
        <input type="text" class="form-control" v-model="percent" placeholder="1.13%" />
      </div>

      <div class="col-md-3">
        Calcular MercadoEnvios:
        <br />
        <select class="form-control" v-model="shipping">
          <option value="true">Si</option>
          <option value="false">No</option>
        </select>
      </div>
    </div>
    <br />
    <button @click="addEach" class="btn btn-success btn-block">Subir productos a Mercadolibre</button>
    <br />
    <br />
    <hr />
    Resultado ({{ total }})
    <br />
    <br />
    <div class="row">
      <div class="col-md-12">
        <table class="table table-bordered table-hover">
          <thead>
            <th>Estado</th>
            <th>Producto</th>
            <th>Mensaje</th>
          </thead>
          <tbody>
            <tr v-for="response_ in response">
              <td>
                <div
                  v-bind:class=" response_.status == 200 ? 'btn btn-success ' : 'btn btn-danger ' "
                >
                  <span v-if="response_.status == 200">APROBADO</span>
                  <span v-else>NO APROBADO</span>
                </div>
              </td>
              <td>{{ response_.title }}</td>
              <td>
                <div v-if="response_.status == 200">
                <b>Tipo de publicación:</b> {{ response_.type }}<br/>
                <b>Código MercadoLibre:</b> {{ response_.code }}<br/>
                <b>Precio MercadoLibre:</b> {{ response_.price }}<br/>
                <b>Porcentaje de recargo aplicado:</b> {{ response_.percent }}
                </div>  
                <div v-else>
                    {{ response_.error.message }}
                </div>
              </td>
            </tr>
          </tbody>
        </table>
      </div>
    </div>
  </div>
</template>

<script>
import axios from "axios";
export default {
  data() {
    return {
      products: [],
      response: [],
      percent: 1.13,
      tag: "",
      type: "",
      shipping: true,
      total: 0
    };
  },
  async mounted() {
    const products = await axios.get("https://meli-nodejs-morano.herokuapp.com/product");
    this.products = products.data;
    console.log(this);
  },
  methods: {
    addEach: async function() {
      await this.$data.products.forEach(async product => {
        product.title = product.title + " " + this.$data.tag;
        const meliPostItem = await axios.post(
          "https://meli-nodejs-morano.herokuapp.com/mercadolibre/item",
          {
            item: product,
            shipping: this.$data.shipping,
            percent: this.$data.percent,
            type: this.$data.type
          }
        );
        console.log(meliPostItem.data.status);
        if (meliPostItem.data.status === 200) {
          this.$data.total += 1;
        }
        await this.$data.response.push(meliPostItem.data);
      });
    }
  }
};
</script> 