<template>
  <div class="container">
    <div class="row">
      <h1>Cori Coins</h1>
      <input
        type="text"
        class="form-control bg-dark text-light rounded-1 border-0 my-4"
        placeholder="Search Coin"
        @keyup="searchCoin()"
        v-model="textSearch"
      />
      <table class="table table-dark">
        <thead>
          <tr>
            <th v-for="title in titles" :key="title">{{ title }}</th>
          </tr>
        </thead>
        <tbody>
          <tr v-for="(coin, index) in filteredCoins" :key="coin.id">
            <td class="text-muted">{{ index + 1 }}</td>
            <td>
              <img :src="coin.image" style="width: 2rem" class="me-3" />
              <span>
                {{ coin.name }}
              </span>
              <span class="ms-3 text-uppercase text-muted">
                {{ coin.symbol }}
              </span>
            </td>
            <td>{{ coin.current_price }} €</td>
            <td
              :class="[
                coin.price_change_percentage_24h > 0
                  ? 'text-success'
                  : 'text-danger',
              ]"
            >
              {{ coin.price_change_percentage_24h }} %
            </td>
            <td>{{ coin.total_volume.toLocaleString() }} €</td>
          </tr>
        </tbody>
      </table>
    </div>
  </div>
</template>

<script>
export default {
  name: "App",
  data() {
    return {
      coins: [],
      filteredCoins: [],
      titles: ["#", "Coin", "Price", "Price Chane", "24 Volume"],
      textSearch: "",
    };
  },
  async mounted() {
    const res = await fetch(
      "https://api.coingecko.com/api/v3/coins/markets?vs_currency=eur&order=market_cap_desc&per_page=100&page=1&sparkline=false"
    );
    const data = await res.json();
    console.log(data);
    this.coins = data;
    this.filteredCoins = data;
  },
  methods: {
    searchCoin() {
      this.filteredCoins = this.coins.filter(
        (coin) =>
          coin.name.toLowerCase().includes(this.textSearch.toLowerCase()) ||
          coin.symbol.toLowerCase().includes(this.textSearch.toLowerCase())
      );
    },
  },
};
</script>

<style>
</style>
