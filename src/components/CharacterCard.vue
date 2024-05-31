<template>
  <div class="character-card">
    <img :src="character.image" alt="Character Image">
    <h2>{{ character.name }}</h2>
    <p>Status: {{ character.status }}</p>
    <p>Species: {{ character.species }}</p>
    <p>Location: {{ character.location.name }}</p>
  </div>
</template>

<script lang="ts">
import { defineComponent } from 'vue';

export default defineComponent({
  props: {
    character: {
      type: Object,
      required: true,
    },
  },
});
</script>

<style scoped>
.character-card {
  border: 1px solid #ddd;
  padding: 10px;
  margin-bottom: 10px;
}
</style>