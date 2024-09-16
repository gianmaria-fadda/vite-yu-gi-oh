<script>
import SelectComponent from './SelectComponent.vue';
import axios from 'axios';

export default {
  data() {
    return {
      allCard: [],
      archetypes: [],
      selectedArchetype: ''
    }
  },
  // 2) Dichiarazione del componente
  components: {
    SelectComponent
  },
  computed: {
    filteredCards() {
      if (this.selectedArchetype) {
        return this.allCard.filter(card => card.archetype === this.selectedArchetype);
      }
      return this.allCard;
    }
  },
  methods: {
    filterByArchetype(archetype) {
      this.selectedArchetype = archetype;
      this.fetchArchetypeCards(archetype);
    },
    fetchArchetypeCards(archetype) {
      axios
        .get(`https://db.ygoprodeck.com/api/v7/cardinfo.php?archetype=${archetype}`)
        .then((res) => {
          this.allCard = res.data.data;
        });
    }
  },
  created() {
    axios
      .get('https://db.ygoprodeck.com/api/v7/cardinfo.php?num=20&offset=0')
      .then((res) => {
        this.allCard = res.data.data;
      });

    axios
      .get('https://db.ygoprodeck.com/api/v7/archetypes.php')
      .then((res) => {
        this.archetypes = res.data;
      });
    }
  }
</script>

<template>
  <main>
    <div class="container">
      <div class="row">
        <div class="col text-start bg-dark text-white py-3">
          Found {{ filteredCards.length }} cards
        </div>
      </div>
    </div>

    <SelectComponent :archetypes="archetypes" @archetype-selected="filterByArchetype"/>

    <div class="container">
      <div class="row">
        <div v-for="(card, i) in filteredCards" :key="i" class="col-3">
          <div>
            <img :src="card.card_images[0].image_url" :alt="card.name" class="w-100">
          </div>

          <b class="text-white">
            {{ card.name }}
          </b>

          <p>
            {{ card.archetype }}
          </p>
        </div>
      </div>
    </div>
  </main>
</template>

<style lang="scss" scoped>

main {
  @include orange-bg;

  .container {
    padding: 0 110px;
    

    .row {
      background-color: white;

      .col-3 {
        @include orange-bg;

        div {
          @include padding-rule;

        }

        p {
          @include padding-rule;

        }
      }

      .col {
        margin: 12px 12px 0 12px;
      }
    }
  }
}
</style>
