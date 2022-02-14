<template>
  <div class="container">
    <div v-show="!showDetails" class="month-picker-container">
      <MonthPickerInput
        v-model="date"
        :defaultYear="2020"
        placeholder="Pick a date range"
        :maxDate="maxDate"
        :minDate="minDate"
        range
      />
      <button @click="clear" class="regular-btn">
        Clear
      </button>
      <button @click="filter" class="regular-btn">
        Filter
      </button>
    </div>
    <div v-if="!showDetails" class="transactions-table">
      <div v-show="showLoader" class="loader">
        loading...
      </div>
      <div v-show="!showLoader && isEmpty" class="empty-collection">
        No transaction records found.
      </div>

      <table v-show="!showLoader && !isEmpty">
        <tr class="transactions-header">
          <th>Date</th>
          <th>Account</th>
          <th>Currency</th>
          <th>Amount</th>
          <th>Status</th>
        </tr>
        <tr
          v-for="transaction in transactions"
          :key="transaction.internalId"
          class="transaction-line"
          @click="openDetails(transaction)"
        >
          <td>{{transaction.transactionDate}}</td>
          <td>{{transaction.account}}</td>
          <td>{{transaction.currency}}</td>
          <td>{{transaction.amount}}</td>
          <td>{{transaction.status}}</td>
        </tr>
      </table>
    </div>

    <div class="details" v-else>
      <div class="details-box">
        <div class="details-header">
          <span @click="dismissDetails" class="back-icon">X</span>
          <h3>Details</h3>
        </div>
        <div class="details-container">
          <div class="transaction-details">
            <h4>ID: </h4>
            <span>{{transactionDetails.id}}</span>
          </div>
          <div class="transaction-details">
            <h4>Category: </h4>
            <span>{{transactionDetails.category}}</span>
          </div>
          <div class="transaction-details">
            <h4>Account: </h4>
            <span>{{transactionDetails.account}}</span>
          </div>
          <div class="transaction-details">
            <h4>Currency: </h4>
            <span>{{transactionDetails.currency}}</span>
          </div>
          <div class="transaction-details">
            <h4>Amount: </h4>
            <span>{{transactionDetails.amount}}</span>
          </div>
          <div class="transaction-details">
            <h4>Created at: </h4>
            <span>{{transactionDetails.createdAt}}</span>
          </div>
          <div class="transaction-details">
            <h4>Description: </h4>
            <span>{{transactionDetails.description}}</span>
          </div>
          <div class="transaction-details">
            <h4>Reference: </h4>
            <span>{{transactionDetails.reference}}</span>
          </div>
          <div class="transaction-details">
            <h4>Status: </h4>
            <span>{{transactionDetails.status}}</span>
          </div>
          <div class="transaction-details">
            <h4>Date: </h4>
            <span>{{transactionDetails.transactionDate}}</span>
          </div>
          <div class="transaction-details">
            <h4>Updated at: </h4>
            <span>{{transactionDetails.updatedAt}}</span>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import { MonthPickerInput } from 'vue-month-picker';
import axios from "axios";

export default {
  name: 'TransactionsTable',
  components: {
    MonthPickerInput
  },
  data() {
    return {
      transactions: [],
      transactionDetails: {},
      showDetails: false,
      showLoader: false,
      date: {},
      mappedDates: []
    }
  },
  async created() {
    await this.loadTransactions();
  },
  computed: {
    maxDate() {
      return new Date(Math.max.apply(null, this.mappedDates));
    },
    minDate() {
      return new Date(Math.min.apply(null, this.mappedDates));
    },
    startDate() {
      return { month: this.getMonths()[this.date.rangeFromMonth], year: this.date.year }
    },
    endDate() {
      return { month: this.getMonths()[this.date.rangeToMonth], year: this.date.year }
    },
    isEmpty() {
      return this.transactions === null || this.transactions.length === 0
    }
  },
  methods: {
    getMonths() {
      return {
        January: "01",
        February: "02",
        March: "03",
        April: "04",
        May: "05",
        June: "06",
        July: "07",
        August: "08",
        September: "09",
        November: "11",
        December: "12",
      }
    },
    openDetails(transaction) {
      this.transactionDetails = transaction;
      this.showDetails = true;
    },
    dismissDetails() {
      this.showDetails = false;
    },
    async clear() {
      if(!this.date.rangeFromMonth) {
        return;
      }

      this.loadTransactions();
      this.date = {};
    },
    async filter() {
      if(!this.date.rangeFromMonth) {
        return;
      }
      this.showLoader = true;
      try {
        // when trying to get a range from a year to another, it's not working (happening for this component, already submitting an issue to it).
        const transactions = await axios.post("http://localhost:8090/v1", {
          query:
            `{transactionsAtRange(start: "${this.startDate.year}-${this.startDate.month}", end: "${this.endDate.year}-${this.endDate.month}" ) {internalId, id, createdAt, updatedAt, transactionDate, account, description, category, reference, currency, amount, status}}`,
        });
        this.transactions = transactions.data.data.transactionsAtRange;
        this.showLoader = false;
      } catch (err) {
        console.log(err);
      }
    },
    async loadTransactions() {
      this.showLoader = true;
      try {
        const transactions = await axios.post("http://localhost:8090/v1", {
          query:
            "{transactions {internalId, id, createdAt, updatedAt, transactionDate, account, description, category, reference, currency, amount, status}}",
        });
        this.transactions = transactions.data.data.transactions;
        this.mappedDates = this.transactions.map(t => new Date(t.transactionDate));
        this.showLoader = false;
      } catch (err) {
        this.showLoader = false;
        this.isEmpty = true;
        console.log(err);
      }
    }
  }
}

</script>

<style scoped>
  .content {
    width: 100%;
    position: relative;
  }

  .month-picker-container>div {
    width: initial;
    padding: 1%;
  }

  .regular-btn {
    margin: 2% 1% 2% 1%;
    cursor: pointer;
    width: 80px;
    border: 1px solid #18515E;
  }

  .transactions-table {
    max-height: 600px;
    overflow-y: scroll;
    border: 2px solid #18515E;
  }

  table {
    width: 100%;
    border-collapse: collapse;
  }

  .transactions-header {
    background-color: gray;
  }

  .transaction:hover {
    cursor: pointer;
    background-color: #18515E;
  }

  th {
    text-align: center;
    padding: 20px 5px;
    color: white;
  }

  td {
    text-align: center;
    padding: 10px 6px 10px 6px;
  }
</style>