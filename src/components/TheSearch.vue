<script setup>
import axios from "axios";
import { ref } from "vue";
import InputText from "primevue/inputtext";
import Button from "primevue/button";
import Card from "primevue/card";
import {store} from '@/lib/store'

const emits = defineEmits(["results", Object]);

const baseUrl = "https://entscheidsuche.ch/_search.php";
let from = 0;
const size = 20;
const areaOfInterests = ref([
  {
    field: "attachment.content",
    keywords: ["Vaud", "Neuchâtel"],
  },
  {
    field: "attachment.content",
    keywords: ["vehicule", "automobile", "voiture", "camion"],
  },
]);

const fetchData = async (from) => {
  store.isLoading = true;
  const requestBody = {
    from: from,
    size: size,
    query: {
      bool: {
        must: areaOfInterests.value.map((aoi) => ({
          bool: {
            should: aoi.keywords.map((keyword) => ({
              match_phrase: { [aoi.field]: keyword },
            })),
            // minimum_should_match: aoi.keywords.length,
            minimum_should_match: 1,
          },
        })),
      },
    },
    sort: {
      date: "desc",
    },
  };

  try {
    const response = await axios.post(baseUrl, requestBody, {
      params: { from, size },
    });
    
    console.log("Données récupérées:", response.data);
    
    emits("results", response.data);
    
  } catch (error) {
    console.error("Erreur lors de la récupération des données:", error);
  }
  store.isLoading = false;
};
</script>

<template>
  <div id="areas-of-interest">
    <Card v-for="(aoi, index) in areaOfInterests" :key="index" class="area-of-interest">
      <template #title>
        <div class="card-header">
          <div>Area of interest {{ index + 1 }}</div>
          <Button @click="areaOfInterests.splice(index, 1)" icon="pi pi-times" />
        </div>
      </template>
      <template #content>
        <div class="keywords">
          <div v-for="(keyword, index) in aoi.keywords" :key="index" class="keyword">
            <InputText :key="index" v-model="aoi.keywords[index]" placeholder="Enter a keyword..." />
            <Button @click="aoi.keywords.splice(index, 1)" icon="pi pi-times" />
            <br />
          </div>
          <Button @click="aoi.keywords.push('')">Add keyword</Button>
        </div>
      </template>
    </Card>
    <Button @click="areaOfInterests.push({ field: 'attachment.content', keywords: [''] })">
      New area of interest
    </Button>
  </div>
  <Button @click="fetchData(from)">Search</Button>
</template>

<style scoped>
#areas-of-interest {
  margin-bottom: 20px;
  width: 100%;
}

.area-of-interest {
  margin-bottom: 20px;
  display: flex;
}

.card-header {
  display: flex;
  justify-content: space-between;
}

.keywords {
  display: flex;
  flex-direction: column;
  gap: 0.5rem;
}
.keyword {
  display: flex;
}
.keyword input {
  flex-grow: 1;
  margin-right: 0.5rem;
}
button {
  justify-content: center;
}
</style>
