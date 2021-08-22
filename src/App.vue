<template>
  <div class="container">
    <div class="row">
      <h1>Precios de CryptoMonedas</h1>

      <input type="text" class="form-control bg-dark text-light rounded-0 border-0 my-4"
      placeholder="Busca una moneda"
      v-model="textSearch"
      @keyup="SearchCoin()"
      autofocus
      >

      <table class="table table-dark">
       <thead>
          <tr>
            <th v-for="titulo in titulos" :key="titulo">
              {{titulo}}
            </th>
          </tr>
        </thead>
        <tbody>
          <tr v-for="(coin,index) in filterCoins" :key="coin.id">
            <td class="text-muted">
              {{index + 1 }}
            </td>

            <td>
              <img :src="coin.image" style="width: 2rem" class="me-2">
              <span>{{coin.name}}</span>
              <span class="ms-2 text-uppercase text-muted">{{coin.symbol}}</span>
            </td>

            <td>
              ${{coin.current_price}}
            </td>

            <td :class="[coin.price_change_percentage_24h > 0 ? 'text-success':'text-danger']">
              {{coin.price_change_percentage_24h}}%
            </td>

            <td>
              ${{coin.total_volume.toLocaleString()}}
            </td>


          </tr>
        </tbody>
      </table>
      <footer class="bg-dark text-white">
        <h2 >by Elihan303  <a href="https://github.com/Elihan303" target="_blank">Github</a></h2>
      </footer>
    </div>
  </div>
</template>

<script>
export default {
  name: "App",
  data() {
    return {
      coins: [],
      filterCoins:[],
      titulos:[
        '#',
        'Coin',
        'Precio',
        'Cambio de precio',
        'Volumen 24h'
      ],
      textSearch: '',
    };
  },
  async mounted() {
    const res = await fetch(
      "https://api.coingecko.com/api/v3/coins/markets?vs_currency=usd&order=market_cap_desc&per_page=100&page=1&sparkline=false"
    );
    const data = await res.json();
    this.coins = data;
    this.filterCoins= data;
  },
  methods:{
    SearchCoin(){
        this.filterCoins = this.coins.filter(
        (coin) => coin.name.toLowerCase().includes(this.textSearch.toLowerCase())||
        coin.symbol.toLowerCase().includes(this.textSearch.toLowerCase())
        );
   
    },
  },

};
</script>

<style>
</style>
