<template>
  <div class="container">
    <div class="month-picker-container">
    </div>
    <div class="loader">
      loading...
    </div>
    <div class="empty-collection">
      No transaction records found.
    </div>

    <div class="table-container">
      <table>
        <tr class="transactions-header">
          <th>Account</th>
          <th>Date</th>
          <th>Category</th>
          <th>Currency</th>
          <th>Amount</th>
          <th>Status</th>
        </tr>
        <tr>
          <td>Fake account</td>
          <td>Fake date</td>
          <td>Fake category</td>
          <td>Fake currency</td>
          <td>Fake amount</td>
          <td>Fake status</td>
        </tr>
      </table>
    </div>

  <div class="details">
    <div class="details-box">
      <div class="details-header">
      </div>
      <div class="details-container">
        <div class="transaction-details">
          <h4>ID: </h4>
          <span>Fake ID</span>
        </div>
        <div class="transaction-details">
          <h4>Category: </h4>
          <span>Fake Account</span>
        </div>
        <div class="transaction-details">
          <h4>Account: </h4>
          <span>Fake Account</span>
        </div>
        <div class="transaction-details">
          <h4>Currency: </h4>
          <span>Fake Currency</span>
        </div>
        <div class="transaction-details">
          <h4>Amount: </h4>
          <span>Fake Amount</span>
        </div>
        <div class="transaction-details">
          <h4>Created at: </h4>
          <span>Fake date</span>
        </div>
        <div class="transaction-details">
          <h4>Description: </h4>
          <span>Fake Description</span>
        </div>
        <div class="transaction-details">
          <h4>Reference: </h4>
          <span>Fake Reference</span>
        </div>
        <div class="transaction-details">
          <h4>Status: </h4>
          <span>Fake Status</span>
        </div>
        <div class="transaction-details">
          <h4>Date: </h4>
          <span>Fake transaction date</span>
        </div>
        <div class="transaction-details">
          <h4>Updated at: </h4>
          <span>Fake Updated at</span>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import { MonthPickerInput } from 'vue-month-picker';

export default {
  name: 'TransactionsTable',
  data() {
    return {
      transactions: [],
      transactionDetails: {},
    }
  },

}

</script>

<style scoped>
  .content {
    width: 100%;
    position: relative;
  }


</style>