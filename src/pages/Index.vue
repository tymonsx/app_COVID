<template>
  <q-page class="full=width flex flex-center">
    <div class="full-width text-center q-gutter-xs q-ma-xs">
      <img style="width:300px; height:300px;" id="imagem" :src="img" />
    </div>
    <div class="q-gutter-xs full=width text-center" style="width:300px;">
      <q-file
        label="selecione uma radiografia"
        outlined
        v-model="arquivo"
        @input="carregarImagem"
      >
        <template v-slot:prepend> </template>
      </q-file>
    </div>
    <div class="full-width text-center q-gutter-sm q-ma-xs">
      <q-btn v-on:click="predizerImagem" color="primary"
        >Reconhecer imagem
      </q-btn>
      <q-btn color="primary" v-on:click="limparCampos">Limpar campos</q-btn>
    </div>
    <div class="textoPredito">{{ msgPredicao }}</div>
  </q-page>
</template>
<style>
.textoPredito {
  white-space: pre-wrap;
  text-align: center;
  margin-top: 2%;
  height: 150px;
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
      objetoPredicao: "",
      modelo: tf.sequential(),
      arquivo: null,
      img: covidExemplo,
      labels: ["COVID-19", "NORMAL"],
      alertaInicial: false
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
        this.modelo = await tf.loadLayersModel("model/model.json");
      } catch (error) {
        alert(error);
      }
      this.msgPredicao = "Modelo carregado";
    },
    carregarImagem() {
      this.img = URL.createObjectURL(this.arquivo);
    },
    predizerImagem() {
      this.msgPredicao = "Aguarde por favor...";
      this.objetoPredicao = document.getElementById("imagem");
      let arrInput = tf.browser.fromPixels(this.objetoPredicao); //
      this.objetoPredicao = tf.image
        .resizeBilinear(arrInput, [224, 224])
        .reshape([1, 224, 224, 3])
        .div(tf.scalar(255));
      let valor = "";
      try {
        valor = this.modelo.predict(this.objetoPredicao);
      } catch (error) {
        alert(error);
      }

      let pcentCovid = (valor.dataSync()[0] * 100).toFixed(4);
      let pcentNormal = (valor.dataSync()[1] * 100).toFixed(4);
      this.msgPredicao =
        this.labels[valor.argMax(-1).dataSync()[0]] +
        "\n" +
        "covid-19: " +
        pcentCovid +
        "%\n" +
        "normal: " +
        pcentNormal +
        "%\n";
    },
    limparCampos() {
      this.arquivo = null;
      this.msgPredicao = "Modelo carregado";
      this.img = covidExemplo;
    }
  }
};
</script>
