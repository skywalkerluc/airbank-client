<template>
  <div id="app">
    <h1 class="title">Transactions</h1>
    <TransactionsTable />
  </div>
</template>

<script>
import TransactionsTable from './components/TransactionsTable.vue'

export default {
  name: 'App',
  components: {
    TransactionsTable
  },
}
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>
