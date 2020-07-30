<template>
  <q-layout view="lHh Lpr lFf">
    <q-header elevated>
      <q-toolbar>
        <q-toolbar-title>
          <div style="width:50px; float:left; ">
            <q-btn to="/" flat style="width:40px">
              <img
                style="display:block; margin-right:auto; margin: 5px; "
                width="40px"
                src="~assets/virus.svg"
              />
            </q-btn>
          </div>
          <div
            style="width:50%; float:left; word-break: break-all; padding-top:10px;  "
          >
            COVID RAIO-X
          </div>
        </q-toolbar-title>

        <div><q-btn icon="info" to="/sobre" flat /></div>
      </q-toolbar>
    </q-header>
    <q-footer elevated>
      <q-toolbar>
        <q-toolbar-title>
          <div style="min-width:40px;width:10%; float:left; ">
            <img style="margin-top: 5px; " width="40px" src="statics/epm.png" />
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
              src="statics/logo_unifesp.png"
            />
          </div>
        </q-toolbar-title>
      </q-toolbar>
    </q-footer>
    <q-page-container>
      <router-view />
    </q-page-container>
  </q-layout>
</template>

<script>
export default {
  name: "MainLayout"
};
</script>
