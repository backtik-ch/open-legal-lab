<script setup>
// import { allData } from '@/assets/data.js';
import DataView from "primevue/dataview";
import Button from "primevue/button";
import ResultCard from "@/components/ResultCard.vue";

import { computed, watchEffect } from "vue";

const props = defineProps({
  data: Object
});
watchEffect(() => {
  console.log(props.data);
});
const totalResult = computed(() => props.data.hits.total.value);
const hits = computed(() => props.data.hits.hits);
for (const hit of hits.value) {
  if (!hit._source.summary) console.log(hit);
}
</script>
<template>
  <div id="container">
    <div class="title">
      <h1>Results ({{ totalResult }})</h1>
    </div>
    <DataView :value="hits" paginator :rows="5">
      <template #list="slotProps">
        <div class="grid grid-nogutter">
          <div v-for="(hit, index) in slotProps.items" :key="index" class="col-12">
            <!-- <ResultCard 
              :summary="hit._source.abstract.fr" 
              :date="hit._source.date"
              :link="hit._source.attachment.content_url" 
              :title="hit._source.title.fr"
              ></ResultCard> -->
            <div class="flex flex-column sm:flex-row sm:align-items-center p-4 gap-3"
              :class="{ 'border-top-1 surface-border': index !== 0 }">
              <!-- Ici, ajustez selon le contenu de vos données -->
              <div class="flex flex-column justify-content-between flex-1 gap-4">
                <div>
                  <h2>{{ hit._source.title.fr }}</h2>
                  <p>Date: {{ hit._source.date }}</p>
                  <p v-if="hit._source.abstract" class="limit-size">
                    Summary: {{ hit._source.abstract.fr }}
                  </p>
                  <a :href="hit._source.attachment.content_url" target="_blank"><Button label="Document Link" /></a>
                </div>
              </div>
            </div>
          </div>
        </div>
      </template>
    </DataView>
  </div>
</template>

<style scoped>
.decision {
  margin-bottom: 20px;
  border-bottom: 1px solid #ccc;
  padding-bottom: 10px;
}

#container {
  width: 60%;
  margin: 0 auto;
  min-width: 500px;
}

.title {
  padding: 0 1.5rem;
}

.limit-size {
  display: -webkit-box;
  -webkit-line-clamp: 3;
  -webkit-box-orient: vertical;
  overflow: hidden;
}
</style>
