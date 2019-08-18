<template>
  <nav class="navbar navbar-expand-lg navbar-light bg-light">
      <router-link to="/" class="navbar-brand">Stock Trader</router-link>
    <button
      class="navbar-toggler"
      type="button"
      data-toggle="collapse"
      data-target="#navbarSupportedContent"
      aria-controls="navbarSupportedContent"
      aria-expanded="false"
      aria-label="Toggle navigation"
      @click="toggleDropdownMenu"
    >
      <span class="navbar-toggler-icon"></span>
    </button>

    <div class="collapse navbar-collapse" :class="{show: isDropdownMenuOpen,}">
      <ul class="navbar-nav">
        <router-link to="/portfolio" activeClass="active" tag="li"><a class="nav-link">Portfolio</a></router-link>
        <router-link to="/stocks" activeClass="active" tag="li"><a class="nav-link">Stocks</a></router-link>
      </ul>
      
      <ul class="navbar-nav ml-auto">
        <li class="nav-item">
          <a class="nav-link" href="#" @click="endDay">End Day</a>
        </li>
        <li class="nav-item dropdown">
          <a
            class="nav-link dropdown-toggle"
            href="#"
            id="navbarDropdown"
            role="button"
            data-toggle="dropdown"
            aria-haspopup="true"
            aria-expanded="false"
            @click="toggleDropdown"
          >Save &#38; Load</a>
          <div class="dropdown-menu" aria-labelledby="navbarDropdown" :class="{show: isDropdownOpen, 'dropdown-menu':true}">
            <a class="dropdown-item" href="#" @click="saveData">Save Data</a>
            <a class="dropdown-item" href="#" @click="loadData">Load Data</a>
            <div class="dropdown-divider"></div>
          </div>
        </li>
      </ul>
      <strong class="navbar-text">Funds: {{ funds | currency }}</strong>
    </div>
  </nav>
</template>

<script>
import {mapActions} from 'vuex';

export default {
  data() {
    return {
      isDropdownOpen : false,
      isDropdownMenuOpen: false,
    }
  },
  computed: {
    funds() {
      return this.$store.getters.funds;
    }
  },
  methods: {
    ...mapActions({
         randomizeStocks: 'randomizeStocks',
         fetchData: 'loadData'
    }),
    endDay() {
      this.randomizeStocks();
    },
    toggleDropdown(isDropdownOpen) {
      this.isDropdownOpen = !this.isDropdownOpen;
    },
    toggleDropdownMenu(isDropdownMenuOpen) {
      this.isDropdownMenuOpen = !this.isDropdownMenuOpen;
    },
    saveData() {
      const data = {
        funds: this.$store.getters.funds,
        stockPortfolio: this.$store.getters.stockPortfolio,
        stocks: this.$store.getters.stocks
      };
      this.$http.put('data.json', data);
    },
    loadData() {
      this.fetchData();
    }
  }
}
</script>