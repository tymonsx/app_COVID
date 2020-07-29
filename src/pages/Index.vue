<template>
  <q-page class="flex flex-center">
    <div class="full-width text-center">
      <img style="width:300px;" id="imagem" :src="img" />
    </div>
    <div class="q-gutter-md text-center" style="width:300px;">
      <q-file outlined v-model="file">
        <template v-slot:prepend>
          <q-icon name="attach_file"></q-icon>
        </template>
      </q-file>
      <button v-on:click="carregaImagem">Carregar</button>
    </div>
    <div class="full-width text-center">
      <button v-on:click="predict">Reconhecer Covid</button>
    </div>
    <div class="element">{{ predictedValue }}</div>
  </q-page>
</template>

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

      this.predictedValue = this.labels[valor.argMax(-1).dataSync()[0]];
    },
    carregaImagem() {
      this.predictedValue = "";
      this.img = URL.createObjectURL(this.file);
      console.log(this.file);
    }
  }
};
</script>
