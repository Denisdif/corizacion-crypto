<template>
  <div class="container">
    <div class="row">

        <h1>Coin Market</h1>

        <input type="text" 
          class="form-control bg-dark text-light rounded-0 border-0 my-4" 
          placeholder="Search Coin"
          v-model="textSearch"
          @keyup="searchCoin()"
           />

        <table class="table table-dark">
          <thead>
            <tr>
              <th v-for="title in titles" :key="title">
                {{title}}
              </th>
            </tr>
          </thead>
          <tbody>
            <tr v-for="(coin, index) in filteredCoins" :key="coin.id">
              <td class="text-muted">
                {{index}}
              </td>
              <td>
                <img :src="coin.image" class="me-2 image">
                <span>
                  {{coin.name}}
                </span>
                <span class="ms-2 text-uppercase text-muted">
                  {{coin.symbol}}
                </span>
              </td>
              <td>
                ${{coin.current_price.toLocaleString()}}
              </td>
              <td :class="[coin.price_change_percentage_24h > 0 ? 'text-success' : 'text-danger']">
                {{coin.price_change_percentage_24h}} %
              </td>
              <td>
                ${{coin.total_volume.toLocaleString()}}
              </td>
            </tr>
          </tbody>
        </table>
    </div>
  </div>

</template>

<script>

export default {
  name: 'App',
  data() {
    return {
      coins: [],
      filteredCoins: [],
      titles:[
        "#",
        "coin",
        "Precio",
        "Cambio de precio",
        "24h Volumen",
      ],
      textSearch: "",
    }
  },
  async mounted() {
    const res = await fetch('https://api.coingecko.com/api/v3/coins/markets?vs_currency=usd&order=market_cap_desc&per_page=100&page=1&sparkline=false')
    const datos = await res.json()
    console.log(datos);
    this.coins = datos;
    this.filteredCoins = datos;
  },
  methods: {
    searchCoin(){
      this.filteredCoins = this.coins.filter(coin => 
        coin.name.toLowerCase().includes(this.textSearch.toLowerCase()) || 
        coin.symbol.toLowerCase().includes(this.textSearch.toLowerCase())
      )
    },
  },

};
</script>

<style>
  .image{
    width: 2rem;
  }
</style>
