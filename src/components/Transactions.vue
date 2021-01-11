<template>
    <table>
        <tr>
            <td>From / To</td>
            <td v-on:click="toggleSort()" id="amount-label">Amount</td>
            <td>Currency</td>
        </tr>
        <tr v-for="(item, index) in transactions"
          class="row"
          :class="{income: item.type === 'income', spending: item.type === 'spending'}"
          :key="index">
          <td>
            <span v-if="item.type === 'income'">{{item.from}}</span>
            <span v-else>{{item.to}}</span>
          </td>
          <td>
          <span> {{item.amount}}</span>
          </td>
          <td>
            <span>{{item.currency}}</span>
          </td>
        </tr>
        <tr>
          <td colspan="3" style="text-align: right;">
            <span>Balance: {{ balance }}<span id="balance"></span> EUR</span>
          </td>
        </tr>
    </table>
</template>

<script>
    export default {
        name: 'HelloWorld',
        props: {
            transactions: Array
        },
        created() {
          this.transactions = this.sortedTransactions;
          this.oldestFirst = false;
        },
        methods: {
          toggleSort: function() {
            this.oldestFirst = !this.oldestFirst;
            let sortedList = this.transactions;
            let order = this.oldestFirst;
            sortedList.sort(function(a, b) {
              if (order) {
                return b.amount - a.amount;
              } else {
                return a.amount - b.amount;
              }
            });
            return sortedList;
          }
        },
        computed: {
          balance: function () {
            let balance = 0;

            for (let i of this.transactions) {
              if (i.type === 'income') {
                balance += i.amount;
              } else {
                balance -= i.amount;
              }
            }
            return balance;
          },

          sortedTransactions: function () {
            return this.toggleSort();
          }
        }
    }
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
    table {
        width: 50%;
        margin: 0 auto;
    }
    tr.income {
      color: green;
    }
    tr.spending {
      color: red;
    }
</style>