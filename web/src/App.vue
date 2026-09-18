<script setup>
import { onMounted } from 'vue';
import Identity from './components/Identity.vue'

onMounted(() => {
  if (import.meta.env.DEV) {
    document.body.classList.remove("none");
  }

  fetch("http://esx_identity/ready", {
    method: "POST",
    body: JSON.stringify({}),
  });

  window.addEventListener("message", (event) => {
    if (event.data.type === "enableui") {
        document.body.classList[event.data.enable ? "remove" : "add"]("none");
    }
  });
})

</script>

<template>
  <div id="vignette" aria-hidden="true"></div>
  <Identity/>
</template>

<style scoped>

</style>
