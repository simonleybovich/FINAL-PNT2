<template>
  <div id="app">
    <h1>Conversor a dólares</h1>
    <div>
      <label for="pesos">Ingrese monto $ </label>
      <input type="number" v-model.number="pesos" id="pesos" placeholder="Ingrese valor en pesos" />
    </div>
    <br>
    <div>
      <label for="dolar">Valor del dolar $ </label>
      <input type="number" v-model.number="dolar" id="dolar" :disabled="actualizacionAutomatica" />
    </div>
    <br>
    <div>
      <label for="actualizacionAutomatica"> - Actualizacion</label>
      <input type="checkbox" v-model="actualizacionAutomatica" id="actualizacionAutomatica" />
    </div>
    <br>
    <div v-if="actualizacionAutomatica">
      <b>{{ ultimaActualizacion }}</b>
    </div>
    <br>
    <h2>Valor convertido USD {{ conversion }}</h2>
  </div>
  <div>
    <h4>respuestas</h4>
    <p>1:b</p>
    <p>2:b</p>
    <p>3:c</p>
    <p>4:b</p>
    <p>5:c</p>
  </div>
</template>

<script>
import axios from 'axios';

export default {
  data() {
    return {
      pesos: 0,
      dolar: 0,
      actualizacionAutomatica: false,
      ultimaActualizacion: null,
    };
  },
  computed: {
    conversion() {
      return (this.pesos / this.dolar).toFixed(2);
    },
  },
  watch: {
    actualizacionAutomatica(valor) {
      if (valor) {
        this.activarActualizacionAutomatica();
      } else {
        clearInterval(this.intervalId);
      }
    },
  },
  methods: {
    async cambioCotizacionDolar() {
      try {
        const response = await axios.get('https://api.bluelytics.com.ar/v2/latest');
        this.dolar = response.data.blue.value_sell;
        this.ultimaActualizacion = new Date().toLocaleString();
      } catch (error) {
        console.error('Error al obtener la cotización del dólar:', error);
      }
    },
    activarActualizacionAutomatica() {
      this.cambioCotizacionDolar();
      this.intervalId = setInterval(this.cambioCotizacionDolar, 2000);
    },
  },
};
</script>

<style scoped>
</style>
