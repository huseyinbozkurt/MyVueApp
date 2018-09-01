<template>
    <div id="app" class="container">
      <div class="field" style="margin-bottom:100px;">
        <div class="control is-large" v-bind:class="{ 'is-loading': isLoading }">
          <input class="input is-large" type="text" placeholder="Type to search" v-on:keyup="getListData($event)">
        </div>
      </div>
      <!-- LIST STARTS HERE -->
      <div class="list">

        <div class="card" v-if="list && list.length > 0" v-for="(item,index) in list" :key='index' style="margin-bottom:15px;">
          <header class="card-header">
            <p class="card-header-title">
              <span>Coin symbol: {{item.symbol}}</span>
              <span class="py-1">-</span>
              <span>Coin Price: {{item.price_usd}}$</span>
            </p>
          </header>
        </div>
        <div v-if="list && list.length === 0">No result found</div>

      </div>
    </div>
</template>

<script>
import Vue from 'vue';

export default {
  name: 'app',
  data: function() {
    return {
      list: null,
      isLoading: false
    };
  },
  created: function() {
    this.getListData();
  },
  methods: {
    getListData: function(event) {
      this.isLoading = true;
      Vue.axios
        .get('https://api.coinmarketcap.com/v1/ticker/')
        .then(
          response => {
            if (event && event.target.value.length > 0) {
              this.list = response.data.filter(item =>
                item.symbol
                  .toLowerCase()
                  .includes(event.target.value.toLowerCase())
              );
            } else {
              this.list = response.data;
            }

            this.isLoading = false;
          },
          error => {
            this.list = [];
            this.isLoading = false;
          }
        )
        .catch(e => {
          this.isLoading = false;
        });
    }
  }
};
</script>

<style>
#app {
  font-family: 'Avenir', Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}

.py-1 {
  margin-left: 5px;
  margin-right: 5px;
}
</style>
