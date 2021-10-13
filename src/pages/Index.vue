<template>
  <q-page class="flex flex-center">
    <div class="full-width text-center q-gutter-xs q-ma-xs">
      <img style="width:300px; height:300px;" id="imagem" :src="img" />
    </div>
    <q-form
      @submit.stop="predizerImagem"
      @reset="limparCampos"
      class="flex flex-center"
    >
      <div class="q-gutter-xs text-center" style="width:224px;">
        <q-file
          outlined
          v-model="arquivo"
          @input="carregarImagem"
          :rules="[val => (val != '' && val != null) || 'Insira uma imagem']"
          accept=".jpg, image/*"
        >
          <template v-slot:prepend>
            <q-icon name="attach_file"></q-icon>
          </template>
        </q-file>
      </div>
      <div class="full-width text-center">
        <div class="element q-ma-xs">{{ msgPredicao }}</div>
      </div>
      <div class="full-width text-center q-gutter-sm q-mt-sm">
        <q-btn color="primary" type="submit">Reconhecer</q-btn>
        <q-btn color="primary" type="reset">Limpar Campos</q-btn>
      </div>
    </q-form>
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
import logo from "assets/logo_virus.png";
export default {
  data() {
    return {
      msgPredicao: "Clique em Reconhecer imagem",
      valorPredito: "",
      modelo: tf.sequential(),
      arquivo: null,
      img: logo,
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
      try {
        this.model = await tf.loadLayersModel("model/model.json");
      } catch (error) {
        alert(error);
      }
      this.msgPredicao = "Modelo carregado";
    },
    carregarImagem() {
      this.img = URL.createObjectURL(this.arquivo);
    },
    alertaCarregando(param) {
      if (param == true) {
        console.log(param);
        this.$q.loading.show({
          spinnerColor: "primary",
          spinnerSize: 140,
          backgroundColor: "white",
          message: "Carregando... Por favor aguarde",
          messageColor: "black"
        });
      } else if (param == false) {
        this.$q.loading.hide();
        console.log(param);
      }
    },
    predizerImagem() {
      this.alertaCarregando(true);
      this.msgPredicao = "Processando...";
      this.valorPredito = document.getElementById("imagem");
      let arrInput = tf.browser.fromPixels(this.valorPredito);
      this.valorPredito = tf.image
        .resizeBilinear(arrInput, [224, 224])
        .reshape([1, 224, 224, 3])
        .div(tf.scalar(255));

      setTimeout(() => {
        let valor = "";

        try {
          valor = this.model.predict(this.valorPredito);
        } catch (error) {
          alert(error);
        }

        let porcentagemCovid = (valor.dataSync()[0] * 100).toFixed(4);
        let porcentagemNormal = (valor.dataSync()[1] * 100).toFixed(4);
        this.msgPredicao =
          this.labels[valor.argMax(-1).dataSync()[0]] +
          "\n\n" +
          "covid-19: " +
          porcentagemCovid +
          "%\n" +
          "normal: " +
          porcentagemNormal +
          "%\n";

        this.alertaCarregando(false);
      }, 500);
    },
    limparCampos() {
      this.arquivo = null;
      this.msgPredicao = "Modelo carregado";
      this.img = logo;
    }
  }
};
</script>
