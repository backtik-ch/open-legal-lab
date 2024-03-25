<script setup>
import TheListResult from "@/components/TheListResult.vue";
import TheSearch from "@/components/TheSearch.vue";

</script>

<template>
  <main id="container">
    <h1>Legal Lens</h1>
    <TheSearch />
    <TheListResult />
  </main>
</template>

<style scoped>
#container {
  width: 60%;
  margin: 0 auto;
  min-width: 500px;
}
</style>
