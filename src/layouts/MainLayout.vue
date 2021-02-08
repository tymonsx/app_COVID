<template>
  <q-layout view="lHh Lpr lFf">
    <q-header reveal bordered class="bg-primary text-white">
      <q-bar class="q-electron-drag electron-only">
        <div>appCovid</div>

        <q-space></q-space>

        <q-btn dense flat icon="minimize" @click="minimize"></q-btn>
        <q-btn dense flat icon="crop_square" @click="maximize"></q-btn>
        <q-btn dense flat icon="close" @click="closeApp"></q-btn>
      </q-bar>
      <q-toolbar>
        <q-toolbar-title
          clickable
          v-ripple
          @click="$router.replace('/')"
          active-class="my-menu-link"
        >
          <div style="width:80px; float:left; ">
            <img
              style="display:block; margin-right:auto;  "
              width="70px"
              src="~assets/virus.svg"
            />
          </div>
          <div
            style="width:50%; margin-left:50%; margin-right:50%; word-break: break-all; padding-top: 20px;"
          >
            <span style="font-size:18px;">App Covid</span>
            <br />
          </div>
        </q-toolbar-title>

        <q-btn icon="info" to="/sobre" flat />
      </q-toolbar>
    </q-header>

    <q-footer elevated>
      <q-toolbar>
        <q-toolbar-title>
          <div style="min-width:40px;width:10%; float:left; ">
            <img style="margin-top: 5px; " width="40px" src="~assets/epm.png" />
          </div>
          <div
            class="desktop-only text-caption text-center float-left"
            style="width:80%"
          >
            Rua Botucatu, 862-Térreo, Vila Clementino, São Paulo (SP). Cep:
            04023-062 - Email: contato@dis.epm.br - CNPJ:60.453.032/0001-74
          </div>
          <div
            class="text-right"
            style="min-width:75px; width:10%;float:right;"
          >
            <img
              style="margin-top: 5px; "
              width="75px"
              src="~assets/logo_unifesp.png"
            />
          </div>
        </q-toolbar-title>
      </q-toolbar>
    </q-footer>
    <q-dialog v-model="alertaInicial">
      <q-card>
        <q-card-section>
          <div class="text-h6 text-center">Atenção</div>
        </q-card-section>

        <q-card-section class="q-pt-none text-justify">
          Esta é uma prova de conceito e não deve ser utilizada como
          diagnóstico. Em caso de sintomas, consulte um médico.
        </q-card-section>

        <q-card-actions align="right">
          <q-btn flat label="OK" color="primary" v-close-popup />
        </q-card-actions>
      </q-card>
    </q-dialog>
    <q-page-container>
      <router-view />
    </q-page-container>
  </q-layout>
</template>

<script>
import Vue from "vue";
import Vuex from "vuex";
Vue.use(Vuex);
export default {
  name: "MainLayout",
  data() {
    return {
      alertaInicial: false
    };
  },
  mounted() {
    this.alertaInicial = true;
  },
  methods: {
    minimize() {
      if (process.env.MODE === "electron") {
        this.$q.electron.remote.BrowserWindow.getFocusedWindow().minimize();
      }
    },
    maximize() {
      if (process.env.MODE === "electron") {
        const win = this.$q.electron.remote.BrowserWindow.getFocusedWindow();
        if (win.isMaximized()) {
          win.unmaximize();
        } else {
          win.maximize();
        }
      }
    },
    closeApp() {
      if (process.env.MODE === "electron") {
        this.$q.electron.remote.BrowserWindow.getFocusedWindow().close();
      }
    }
  }
};
</script>
