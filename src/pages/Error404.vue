<template>
  <div
    class="fullscreen bg-blue text-white text-center q-pa-md flex flex-center"
  >
    <div>
      <div style="font-size: 30vh">
        Erro 404
      </div>

      <div class="text-h2" style="opacity:.4">
        Página Não Encontrada
      </div>

      <q-btn
        class="q-mt-xl"
        color="white"
        text-color="blue"
        unelevated
        to="/"
        label="Voltar"
        no-caps
      />
    </div>
  </div>
</template>

<script>
export default {
  name: "Error404"
};
</script>
