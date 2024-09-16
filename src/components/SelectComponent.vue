<script>
import axios from 'axios';

export default {
  data() {
    return {
      selectedArchetype: ''
    }
  },
  props: {
    archetypes: Array
  },
  methods: {
    fetchArchetypeCards() {
      if (this.selectedArchetype) {
        axios.get(`https://db.ygoprodeck.com/api/v7/cardinfo.php?archetype=${this.selectedArchetype}`)
          .then((res) => {
            this.$emit('archetype-selected', this.selectedArchetype);
          });
      }
    }
  }
}
</script>

<template>
  <select v-model="selectedArchetype" @change="fetchArchetypeCards">
    <option value="">Seleziona qui l'archetipo...</option>
    <option v-for="archetype in archetypes" :key="archetype" :value="archetype.archetype_name">
      {{ archetype.archetype_name }}
    </option>
  </select>
</template>

<style lang="scss" scoped>
select {
  padding: 10px;
  font-size: 16px;
  margin: 10px 0;
}
</style>
