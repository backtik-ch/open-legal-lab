<script setup>
// import { allData } from '@/assets/data.js';
import DataView from "primevue/dataview";
import Button from "primevue/button";
import ResultCard from "@/components/ResultCard.vue";
import Skeleton from 'primevue/skeleton';
import {store} from '@/lib/store'

import { computed, watchEffect } from "vue";

const props = defineProps({
  data: Object
});
watchEffect(() => {
  console.log(props.data);
});
const totalResult = computed(() => props.data ? props.data.hits.total.value : 0);
const hits = computed(() => props.data ? props.data.hits.hits : []);
for (const hit of hits.value) {
  if (!hit._source.summary) console.log(hit);
}
</script>
<template>
    <div id="complete-loading" v-if="store.isLoading">
      <div class="title">
        <h1>Searching...</h1>
      </div>
      <div class="loading-state-container">
        <div>
          <div class="header">
            <Skeleton width="10rem" class="title" padding="5px"></Skeleton>
            <Skeleton width="5rem" class="date"></Skeleton>
          </div>
        </div>
        <Skeleton width="100%" height="150px"></Skeleton>
        <div class="flex justify-content-between mt-3">
          <Skeleton width="8rem" height="2rem"></Skeleton>
        </div>
      </div>
    </div>
    <div class="data" v-if="!store.isLoading && data">
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

.loading-state-container {
  display: flex;
  justify-content: space-between;
  gap: 10px;
  flex-direction: column;
}

.loading-state-container .header {
  display: flex;
    justify-content: space-between;
    gap: 10px;
    flex-direction: column;
}

</style>
