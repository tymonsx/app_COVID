<template>
  <q-layout view="lHh Lpr lFf">
    <q-header elevated>
      <q-toolbar>
        <q-toolbar-title>
          <div style="width:50px; float:left; ">
            <img
              style="display:block; margin-right:auto; margin: 5px; "
              width="40px"
              src="~assets/virus.svg"
            />
          </div>
          <div
            style="width:50%; float:left; word-break: break-all; padding-top:10px;  "
          >
            COVID RAIO-X
          </div>
        </q-toolbar-title>

        <div>Quasar v{{ $q.version }}</div>
      </q-toolbar>
    </q-header>

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
