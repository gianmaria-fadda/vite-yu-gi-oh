<script>
import axios from 'axios';

export default {
  data() {
    return {
      allCard: []
    }
  },
  created() {
    axios
      .get('https://db.ygoprodeck.com/api/v7/cardinfo.php?num=20&offset=0')
      .then((res)=> {
        console.log(res.data.data);
        
        this.allCard = res.data.data;
      });
  }
}
</script>

<template>
  <main>
    <div class="container">
      <div class="row">
        <div class="col text-start bg-dark text-white py-3">
          Found {{ allCard.length }} cards
        </div>
      </div>
    </div>
    <div class="container">
      <div class="row">
        <div v-for="(card, i) in allCard" :key="i" class="col-3">
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
