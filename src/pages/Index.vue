<template>
  <q-page class="flex flex-center">
    <div class=" q-pt-lg">
      <div class="full-width text-center">
        <img style="width:300px;" id="imagem" :src="img" />
      </div>
      <div class="full-width q-ma-md text-center" style="width:300px;">
        <q-file
          label="selecione uma radiografia"
          outlined
          v-model="file"
          @input="carregarImagem"
        >
          <template v-slot:prepend> </template>
        </q-file>
        <br />
        <q-btn v-on:click="predizerImagem" color="primary" class="full-width"
          >Reconhecer</q-btn
        >
      </div>
      <div class="textoPredito">{{ msgPredicao }}</div>
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
      msgPredicao: "Clique em Reconhecer imagem",
      valorPredito: "",
      model: tf.sequential(),
      file: null,
      img: covidExemplo,
      labels: ["COVID-19", "NORMAL"]
    };
  },

  mounted() {
    try {
      window.fetch = fetchPolyfill;
      this.carregarModelo();
    } catch (error) {
      alert(error);
    }
  },
  methods: {
    async carregarModelo() {
      this.msgPredicao = "Carregando modelo ...";
      try {
        this.model = await tf.loadLayersModel("model/model.json");
      } catch (error) {
        alert(error);
      }
      this.msgPredicao = "Modelo carregado";
    },
    carregarImagem() {
      //this.msgPredicao = "Modelo carregado";
      this.img = URL.createObjectURL(this.file);
    },
    predizerImagem() {
      this.msgPredicao = "Processando...";
      this.valorPredito = document.getElementById("imagem");
      let arrInput = tf.browser.fromPixels(this.valorPredito); //
      this.valorPredito = tf.image
        .resizeBilinear(arrInput, [224, 224])
        .reshape([1, 224, 224, 3])
        .div(tf.scalar(255));
      let valor = "";
      try {
        valor = this.model.predict(this.valorPredito);
      } catch (error) {
        alert(error);
      }

      let pcentCovid = (valor.dataSync()[0] * 100).toFixed(4);
      let pcentNormal = (valor.dataSync()[1] * 100).toFixed(4);
      this.msgPredicao =
        this.labels[valor.argMax(-1).dataSync()[0]] +
        "\n\n" +
        "covid-19: " +
        pcentCovid +
        "%\n" +
        "normal: " +
        pcentNormal +
        "%\n";
    }
  }
};
</script>
