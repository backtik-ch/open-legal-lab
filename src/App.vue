<script setup>
import TheListResult from "@/components/TheListResult.vue";
import TheSearch from "@/components/TheSearch.vue";
import { ref } from "vue";

const data = ref(null)

</script>

<template>
  <main>
    <h1>Page title</h1>
    <TheSearch @results="data = $event" />
    <TheListResult v-if="data" :data="data" />
  </main>
</template>

<style scoped>

</style>
