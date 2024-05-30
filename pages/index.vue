<template>
  <div class="background">
    <div class="container">
      <div class="search-container">
        <input type="text" v-model="searchQuery" class="input" />
        <button @click="searchData" v-if="!loader" class="button is-primary">
          Wyszukaj
        </button>
        <button @click="searchData" disabled v-else class="button is-primary">
          Wyszukaj
        </button>
        <div class="loader" v-if="loader">To jest ladowanie danych</div>
      </div>

      <div class="columns is-multiline">
        <div
          v-for="(image, index) in resultQuery"
          :key="index"
          class="column is-one-quarter"
        >
          <figure class="image is-3by2">
            <img
              :src="image.webformatURL"
              :alt="'Image ' + index"
              :class="{ 'image-border': image.type !== 'photo' }"
            />
          </figure>
        </div>
      </div>
    </div>
  </div>
</template>

<script setup>
import { ref, onBeforeMount } from "vue";

const searchQuery = ref("storm");
const resultQuery = ref([]);
const loader = ref(false);

const searchData = async () => {
  loader.value = true;
  const apiKey = "44151461-1a202fa70548d0f0141e3620a";
  const url = `https://pixabay.com/api/?key=${apiKey}&q=${encodeURIComponent(
    searchQuery.value
  )}`;

  try {
    const response = await fetch(url);
    const data = await response.json();

    if (data.totalHits > 0) {
      resultQuery.value = data.hits;
    } else {
      console.log("No hits");
    }
  } catch (error) {
    console.error("Error fetching data:", error);
  } finally {
    loader.value = false;
  }
};

onBeforeMount(async () => {
  await searchData();
});
</script>

<style scoped>
.background {
  background: linear-gradient(to bottom right, #7e47c7, #42b7df);
  min-height: 100vh;
}

.container {
  padding: 20px;
  max-width: 1200px;
  margin: 0 auto;
}

.search-container {
  margin-bottom: 20px;
}

.columns {
  column-gap: 20px;
}

.image {
  border-radius: 5px;
  overflow: hidden;
  background: white;
  box-shadow: 0px 4px 8px rgba(0, 0, 0, 0.1); /* Dodaj cień */
}

.image-border {
  border: 3px solid rgb(255, 255, 255); /* Grubość obramowania */
  border-radius: 5px; /* Zakrzywienie rogów obramowania */
}
</style>
