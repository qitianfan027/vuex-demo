<template>
  <div class="coinListWrapper">
    <h1 class="coinListTitle">Coins</h1>

    <div class="loader"
         v-if="fetchCoinsTask.spinning"></div>
    <div class="error"
         v-if="fetchCoinsTask.error">
      {{ fetchCoinsTask.error }}
    </div>
    <ul class="coinList"
        v-else-if="fetchCoinsTask.hasValue">
      <li v-for="coin in coins"
          :key="coin.id">
        <span>{{ coin.name }}</span>

        <span>{{ coin.quotes.USD.price | currency }}</span>

        <div class="actions">
          <font-awesome-icon v-if="coin.inPortfolio"
                             size="lg"
                             icon="check"
                             class="action added" />
          <font-awesome-icon v-else
                             size="lg"
                             icon="cart-plus"
                             @click="addToPortfolio(coin)"
                             class="clickable action add" />
        </div>
      </li>
    </ul>
  </div>
</template>

<script>
import { mapActions, mapState } from 'vuex'

export default {
  computed: {
    ...mapState(['coins', 'fetchCoinsTask'])
  },

  methods: {
    ...mapActions(['fetchCoins', 'addToPortfolio'])
  },

  created() {
    this.fetchCoins()
  }
}
</script>


<style lang="scss" scoped>
@import '../../lib/styles/coinList';
@import '../../lib/styles/loader';
</style>
