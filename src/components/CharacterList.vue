<template>
  <div>
    <input type="text" v-model="nameFilter" placeholder="Filter by name">
    <select v-model="statusFilter">
      <option value="">All</option>
      <option value="Alive">Alive</option>
      <option value="Dead">Dead</option>
      <option value="unknown">Unknown</option>
    </select>
    <button @click="applyFilters">Apply</button>
    <div v-for="character in filteredCharacters" :key="character.id">
      <character-card :character="character" />
    </div>
    <button @click="loadMoreCharacters">Load More</button>
  </div>
</template>

<script lang="ts">
import { defineComponent, ref, watch } from 'vue';
import axios from 'axios';
import CharacterCard from './CharacterCard.vue';

interface Character {
  id: number;
  name: string;
  status: string;
}


export default defineComponent({
  components: {
    CharacterCard,
  },
  setup() {
    const characters = ref<Character[]>([]);
    const filteredCharacters = ref<Character[]>([]);
    const page = ref(1);
    const nameFilter = ref('');
    const statusFilter = ref('');

    const loadCharacters = async () => {
      const response = await axios.get(`https://rickandmortyapi.com/api/character?page=${page.value}`);
      characters.value = [...characters.value, ...response.data.results];
      applyFilters();
    };

    const applyFilters = () => {
      filteredCharacters.value = characters.value.filter((character: any) => {
        return (
            character.name.toLowerCase().includes(nameFilter.value.toLowerCase()) &&
            (statusFilter.value === '' || character.status.toLowerCase() === statusFilter.value.toLowerCase())
        );
      });
    };

    const loadMoreCharacters = () => {
      page.value++;
      loadCharacters();
    };

    loadCharacters();

    return {
      filteredCharacters,
      nameFilter,
      statusFilter,
      loadMoreCharacters,
      applyFilters,
    };
  },
});
</script>