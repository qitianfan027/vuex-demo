<template>
  <div class="coinListWrapper">
    <h1 class="coinListTitle">Portfolio</h1>

    <ul class="coinList">
      <li v-for="coin in portfolio" :key="coin.id">
        <span>{{ coin.name }}</span>

        <span>{{ coin.quotes.USD.price | currency }}</span>

        <span>
          <template v-if="!transactionTask.pending({coin})">
            {{ coin.amountOwned }} {{ coin.symbol }} owned
          </template>

          <template v-if="transactionTask.spinning({coin})">
            ...
          </template>
        </span>

        <div class="actions">
          <font-awesome-icon size="lg" icon="plus-square" @click="initBuy(coin)" class="clickable action buy" />
          <font-awesome-icon size="lg" icon="minus-square" @click="initSell(coin)" class="clickable action sell" />
          <font-awesome-icon size="lg" icon="trash-alt" @click="removeFromPortfolio(coin)" class="clickable action trash" />
        </div>
      </li>
    </ul>

    <div class="portfolioValue">
      Portfolio Value: {{ portfolioValue | currency }}
    </div>
  </div>
</template>

<script>
import { mapActions, mapGetters, mapState } from 'vuex'

export default {
  computed: {
    ...mapState(['transactionTask']),
    ...mapGetters(['portfolio', 'portfolioValue'])
  },

  methods: {
    ...mapActions(['removeFromPortfolio', 'buy', 'sell']),

    initBuy(coin) {
      const message = `How much ${coin.symbol} would you like to buy?`
      this.buy({ coin, amount: prompt(message) })
    },

    initSell(coin) {
      const message = `How much ${coin.symbol} would you like to sell?`
      this.sell({ coin, amount: prompt(message) })
    }
  }
}
</script>


<style lang="scss" scoped>
@import '../../lib/styles/coinList';

.portfolioValue {
  font-weight: 600;
  padding: 10px 0;
}
</style>
