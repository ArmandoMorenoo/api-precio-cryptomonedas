<template>
  <div class="container">
    <div class="">
      <!-- Título de la página -->
      <div class="text-center">
        <h1 class="text-center px-5 py-2 titulo">Precio de Critomonedas</h1>
      </div>

      <!-- Barra de búsqueda -->
      <div class="w-100 text-center">
        <input
          type="text"
          class="border-0 my-4 input"
          placeholder="Buscar Criptomoneda"
          @keyup="buscarCriptomoneda()"
          v-model="textSearch"
        />
      </div>

      <!-- Tabla: Contenido principal -->
      <table class="tabla">
        <!-- Cabecera de la tabla -->
        <thead>
          <tr>
            <th v-for="title in titles" :key="title" class="titulos-tabla">{{ title }}</th>
          </tr>
        </thead>

        <!-- Body de la tabla -->
        <tbody>
          <tr v-for="(coin, index) in filteredCoins" :key="coin.id">
            <!-- Enumeración de las criptomonedas -->
            <td class="">{{ index + 1 }}</td>

            <!-- Icono de las criptomonedas -->
            <td>
              <img :src="coin.image" alt="" style="width: 2rem" class="me-2" />
              <!-- Nombre de cada criptomoneda -->
              <span>{{ coin.name }}</span>
              <!-- Símbolo de cada criptomoneda -->
              <span class="ms-2 text-uppercase">
                <b>{{ coin.symbol }}</b>
              </span>
            </td>

            <!-- Precio de las criptomonedas -->
            <td><sup>$</sup> {{ coin.current_price }}</td>

            <!-- Porcentaje de crecimiento de las criptomonedas -->
            <td
              :class="[
                coin.price_change_percentage_24h > 0
                  ? 'text-success'
                  : 'text-danger',
              ]"
            >
              {{ coin.price_change_percentage_24h.toFixed(2) }} %
            </td>

            <!-- Volumen las últimas 24 horas -->
            <td><sup>$</sup> {{ coin.total_volume.toLocaleString() }}</td>
          </tr>
        </tbody>

      </table>

      <!-- Footer -->
      <footer class="m-4 text-center">
        <span class="w-100 text-center"
          >API consumida desde
          <b>
            <a href="https://www.coingecko.com/es" class="text-decoration-none text-reset text-center" target="_black">
              CoinGecko
            </a>
          </b>
        </span>
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
      filteredCoins: [],
      titles: [
        "N°",
        "Criptomoneda",
        "Precio",
        "% de crecimiento",
        "Volumen en 24h",
      ],
      textSearch: "",
    };
  },

  async mounted() {
    const res = await fetch(
      "https://api.coingecko.com/api/v3/coins/markets?vs_currency=usd&order=market_cap_desc&per_page=100&page=1&sparkline=false"
    );
    const data = await res.json();
    // console.log(data);
    this.coins = data;
    this.filteredCoins = data;
  },
  methods: {
    buscarCriptomoneda() {
      this.filteredCoins = this.coins.filter(
        (coin) =>
          coin.name.toLowerCase().includes(this.textSearch.toLowerCase()) ||
          coin.symbol.toLowerCase().includes(this.textSearch.toLowerCase())
      );
    },
  },
};
</script>

<style></style>
