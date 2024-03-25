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
    <template>
      <div class="card">
        <DataView :value="hits" paginator :rows="5">
          <template #list="slotProps">
            <div class="grid grid-nogutter">
              <div v-for="(hit, index) in slotProps.items" :key="index" class="col-12">
                <div class="flex flex-column sm:flex-row sm:align-items-center p-4 gap-3"
                  :class="{ 'border-top-1 surface-border': index !== 0 }">
                  <!-- <div class="md:w-10rem relative">
                    <img class="block xl:block mx-auto border-round w-full"
                      :src="`https://primefaces.org/cdn/primevue/images/product/${item.image}`" :alt="item.name" />
                    <Tag :value="item.inventoryStatus" :severity="getSeverity(item)" class="absolute"
                      style="left: 4px; top: 4px"></Tag>
                  </div> -->
                  <div class="flex flex-column md:flex-row justify-content-between md:align-items-center flex-1 gap-4">
                    <div class="flex flex-row md:flex-column justify-content-between align-items-start gap-2">
                      <div>
                        <div class="text-lg font-medium text-900 mt-2">{{ hit._source.title.fr }}</div>
                        <span class="font-medium text-secondary text-sm">{{ hit._source.date }}</span>
                        <div v-if="hit._source.abstract" class="text-lg font-medium text-900 mt-2">
                          {{ hit._source.abstract.fr }}
                        </div>
                      </div>
                      <!-- <div class="surface-100 p-1" style="border-radius: 30px">
                        <div class="surface-0 flex align-items-center gap-2 justify-content-center py-1 px-2"
                          style="border-radius: 30px; box-shadow: 0px 1px 2px 0px rgba(0, 0, 0, 0.04), 0px 1px 2px 0px rgba(0, 0, 0, 0.06)">
                          <span class="text-900 font-medium text-sm">{{ item.rating }}</span>
                          <i class="pi pi-star-fill text-yellow-500"></i>
                        </div>
                      </div> -->
                    </div>
                    <div class="flex flex-column md:align-items-end gap-5">
                      <!-- <span class="text-xl font-semibold text-900">${{ item.price }}</span> -->
                      <div class="flex flex-row-reverse md:flex-row gap-2">
                        <a :href="() => hit._source.attachment.content_url ?: `#`"><Button icon="pi pi-shopping-cart" label="Download PDF"
                            :disabled="!hit._source.attachment.content_url"
                            class="flex-auto md:flex-initial white-space-nowrap"></Button></a>
                      </div>
                    </div>
                  </div>
                </div>
              </div>
            </div>
          </template>
        </DataView>
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
