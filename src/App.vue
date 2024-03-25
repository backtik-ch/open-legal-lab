<script setup>
import TheListResult from "@/components/TheListResult.vue";
import TheSearch from "@/components/TheSearch.vue";
import { ref } from "vue";

const data = ref(null)

</script>

<template>
  <main id="container">
    <h1>Page title</h1>
    <TheSearch @results="data = $event" />
    <TheListResult :data="data" />
  </main>
</template>

<style scoped>
#container {
  width: 60%;
  margin: 0 auto;
  min-width: 500px;
}
</style>
