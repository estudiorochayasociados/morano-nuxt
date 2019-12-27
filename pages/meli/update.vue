<template>
  <div class="container">
    <h2>TOTAL DE PRODUCTOS A AGREGAR: {{ products.length }}</h2>
    <button @click="addEach" class="btn btn-success btn-block">Modificar productos a Mercadolibre</button>
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
            <tr v-for="response_ in response" >
              <td>

                <div v-bind:class=" response_.status == 200 ? 'btn btn-success ' : 'btn btn-danger ' ">
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
                    <b>Error:</b> {{ response_.error.message }}
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
      total: 0
    };
  },
  async mounted() {
    const products = await axios.get("https://meli-nodejs-morano.herokuapp.com/product");
    this.products = products.data;    
  },
  methods: {
    addEach: async function() {
      await this.$data.products.forEach(async product => {
        await product.mercadolibre.forEach(async meli => {
          const meliPostItem = await axios.put(
            "https://meli-nodejs-morano.herokuapp.com/mercadolibre/item/" + meli.code,
            {
              item: product,
              shipping: meli.shipping,
              percent: meli.percent,
              type: meli.type
            }
          );
          console.log(meliPostItem);
          if (meliPostItem.data.status === 200) {
            this.$data.total += 1;
          }
          await this.$data.response.push(meliPostItem.data);
        });
      });
    }
  }
};
</script> 