<template>
  <q-page class="flex flex-center">
    <div class=" q-pt-lg">
      <div class="full-width text-center ">
        <div class="text-h6 ">Sobre</div>
        <div class="text-justify q-ma-md">
          Este aplicativo foi criado como exemplo do uso conjunto das seguintes
          ferramentas e bibliotecas: python, tensorflow, tensorflowjs, quasar,
          vue, cordova.
          <br /><br />
          Primeiramente foi treinado uma rede neural em python utilizando o
          tensorflow. Então o modelo treinado foi salvo no formato h5
          <br /><br />

          <span
            @click="
              abrirSites('https://github.com/tymonsx/raiox_covid_python.git')
            "
          >
            (código disponível em
            https://github.com/tymonsx/raiox_covid_python.git)
          </span>
          <br /><br />
          Em seguida o modelo foi convertido para .json e o treino para .bin com
          o tensorflowjs_wizard
          <br /><br />
          Então foi importado para o projeto no quasar utilizando VUE e o
          tensorflowjs
          <br /><br />

          <span @click="abrirSites('https://github.com/tymonsx/app_COVID.git')">
            (código disponível em https://github.com/tymonsx/app_COVID.git)
          </span>
          <br /><br />
          Finalmente foi exportado para cordova pelas ferramentas do quasar como
          explicado na documentação.
          <br /><br />

          <span
            @click="
              abrirSites(
                'https://quasar.dev/quasar-cli/developing-cordova-apps/preparation'
              )
            "
          >
            (disponível em
            https://quasar.dev/quasar-cli/developing-cordova-apps/preparation)
          </span>
        </div>
      </div>
    </div>
  </q-page>
</template>

<script>
import { openURL } from "quasar";
export default {
  name: "PageSobre",
  methods: {
    abrirSites(param) {
      openURL(param);
    }
  }
};
</script>
