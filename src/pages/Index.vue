<template>
  <q-page class="flex flex-center">
    <div class=" q-pt-lg">
      <div class="full-width text-center ">
        <img style="width:300px;" id="imagem" :src="img" />
      </div>
      <div class="full-width q-ma-md text-center" style="width:300px;">
        <q-file outlined v-model="file">
          <template v-slot:prepend> </template>
        </q-file>
        <br />
        <q-btn
          v-on:click="carregaImagem"
          color="primary"
          :class="$q.screen.width > 1000 ? '' : 'full-width'"
          :style="$q.screen.width > 1000 ? 'width:49.5%; ' : 'margin-right:5px'"
          >Carregar</q-btn
        >

        <q-btn
          v-on:click="predict"
          color="primary"
          :class="$q.screen.width > 1000 ? '' : 'full-width'"
          :style="$q.screen.width > 1000 ? 'width:49.5%;' : ''"
          >Reconhecer</q-btn
        >
      </div>
      <div class="textoPredito">{{ predictedValue }}</div>
    </div>
  </q-page>
</template>
<style>
.textoPredito {
  white-space: pre-wrap;
  text-align: center;
  height: 200px;
}
</style>
<script>
import * as tf from "@tensorflow/tfjs";
import { fetch as fetchPolyfill } from "whatwg-fetch";
import covidExemplo from "assets/covid.jpeg";
export default {
  name: "PageIndex",
  data() {
    return {
      predictedValue: "Clique em Reconhecer imagem",
      valueToPredict: "",
      model: tf.sequential(),
      file: null,
      img: covidExemplo,
      labels: ["COVID-19", "NORMAL"]
    };
  },

  mounted() {
    try {
      window.fetch = fetchPolyfill;
      this.carregar_modelo();
    } catch (error) {
      alert(error);
    }
    //URL.createObjectURL(file);
  },
  methods: {
    async carregar_modelo() {
      this.predictedValue = "Carregando Modelo ...";
      try {
        this.model = await tf.loadLayersModel("model/model.json");
      } catch (error) {
        alert(error);
      }
      this.predictedValue = "Modelo Carregado";
    },
    predict() {
      this.predictedValue = "Processando...";
      this.valueToPredict = document.getElementById("imagem");
      let arrInput = tf.browser.fromPixels(this.valueToPredict); //
      this.valueToPredict = tf.image
        .resizeBilinear(arrInput, [224, 224])
        .reshape([1, 224, 224, 3]);
      let valor = "";
      try {
        valor = this.model.predict(this.valueToPredict);
      } catch (error) {
        alert(error);
      }

      //this.predictedValue = this.labels[valor.argMax(-1).dataSync()[0]];
      let pcentCovid = (valor.dataSync()[0] * 100).toFixed(4);
      let pcentNormal = (valor.dataSync()[1] * 100).toFixed(4);
      this.predictedValue =
        this.labels[valor.argMax(-1).dataSync()[0]] +
        "\n\n" +
        "covid-19: " +
        pcentCovid +
        "%\n" +
        "normal: " +
        pcentNormal +
        "%\n";
    },
    carregaImagem() {
      this.predictedValue = "";
      this.img = URL.createObjectURL(this.file);
      console.log(this.file);
    }
  }
};
</script>
