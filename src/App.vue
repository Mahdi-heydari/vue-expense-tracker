<template>
  <Header />
  <div class="container">
    <Balance :total="+total" />
    <IncomeExpenses :income="+income" :expense="+expense" />
    <TransactionList :transactions="transactions" @transactionDeleted="handleTransactionDeleted" />
    <AddTransaction @transactionSubmitted="handleTransactionSubmitted" />
  </div>
</template>

<script setup>
import { ref, computed, onMounted  } from "vue";

import Header from "./components/Header.vue";
import Balance from "./components/Balance.vue";
import IncomeExpenses from "./components/IncomeExpenses.vue";
import TransactionList from "./components/TransactionList.vue";
import AddTransaction from "./components/AddTransaction.vue";

const transactions = ref([]);

onMounted(() => {
  const savedTransactions = JSON.parse(localStorage.getItem('transactions'));

  if (savedTransactions) {
    transactions.value = savedTransactions;
  }
});

// Get TOTAL
const total = computed(() => {
  return transactions.value.reduce((acc, transaction) => { return acc + transaction.amount}, 0)
});

// get INCOME
const income = computed(() => {
  return transactions.value.filter(transaction => transaction.amount > 0).reduce((acc, transaction) => { return acc + transaction.amount}, 0).toFixed(2)
});

// get EXPENSE
const expense = computed(() => {
  return transactions.value.filter(transaction => transaction.amount < 0).reduce((acc, transaction) => { return acc + transaction.amount}, 0).toFixed(2)
});

// add & submit transaction 
const handleTransactionSubmitted = (transactionData) => {
  transactions.value.push({
    id:generateUniqueId(),
    text:transactionData.text,
    amount:transactionData.amount,
  });

  saveTransactionsToLocalStorage();
}
const generateUniqueId = () => {return crypto.randomUUID()};

// remove & delete transaction
const handleTransactionDeleted = (id) => {
  transactions.value = transactions.value.filter(transaction => transaction.id !== id)
  saveTransactionsToLocalStorage();
}

// Save transactions to local storage
const saveTransactionsToLocalStorage = () => {
  localStorage.setItem('transactions', JSON.stringify(transactions.value));
};

</script>
