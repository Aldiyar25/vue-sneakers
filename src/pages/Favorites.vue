<script setup>
import { ref, onMounted } from "vue";
import axios from "axios";
import CardList from "../components/CardList.vue";

const favorites = ref([]);

onMounted(async () => {
  try {
    const { data } = await axios.get(
      "https://742c758d2468301e.mokky.dev/favorites"
    );
    favorites.value = data.map((obj) => obj.item);
    console.log(data);
  } catch (err) {
    console.log(err);
  }
});

const handleFavoriteClick = (id, isFavorite) => {
  if (isFavorite) {
    favorites.value.push(id);
  } else {
    favorites.value = favorites.value.filter((item) => item !== id);
  }
};
</script>

<template>
  <CardList :items="favorites" @addToFavorite="handleFavoriteClick" />
</template>
