<script setup>
// import { allData } from '@/assets/data.js'; 
import DataView from 'primevue/dataview';
import Button from 'primevue/button';
import { ref } from 'vue';
import DataViewLayoutOptions from 'primevue/dataviewlayoutoptions'

const props = defineProps({
  title : String,
  date: String,
  summary: String,
  link: String,
})
console.log(props)
function convertDate(theDate) {
  let dateParts = theDate.split("-");
  let convertedDate = `${dateParts[2]}.${dateParts[1]}.${dateParts[0]}`;
  return convertedDate;
}


</script>
<template>
  <div id="container">
    <div class="header-container">
      <h1 class="title">{{props.title}}</h1>
      <h2 class="subtitle">{{ convertDate(props.date) }}</h2>
    </div>
    <div class="text-container">
      <p>{{ props.summary }}</p>
    </div>
    <div class="button-container">
      <a :href="props.link" target="_blank"><Button label="Download PDF" /></a>
    </div>
  </div>
</template>


<style scoped>
#container {
  font-family: var(--font-family);
  color: var(--text-color);
  /* width: 60%;
  margin: 0 auto;
  min-width: 500px; */
  padding-bottom: 10px;
  border-bottom: 1px solid rgb(74, 75, 78);
}

.title-container {
  display: flex;
  justify-content: space-between;
}

.title {
  font-size: 16px;
  margin : 5px 0;
}

.subtitle {
  font-size: 12px;
  color: var(--bluegray-500);
  margin: 0;
  padding: 0;
}
.limit-size {
  display: -webkit-box;
    -webkit-line-clamp: 3;
    -webkit-box-orient: vertical;
    overflow: hidden;
}

.text-container p {
  margin: 5px 0;
  font-size: 12px;
}

.button-container {
  text-align: right
}
</style>
