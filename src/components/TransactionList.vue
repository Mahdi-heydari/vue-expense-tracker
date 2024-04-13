<template>
  <h3>History</h3>
  <ul id="list" class="list">
    <li
      v-for="transaction in transactions"
      :key="transaction.id"
      :class="transaction.amount > 0 ? 'plus' : 'minus'"
    >
      {{ transaction.text }} <span>${{ transaction.amount }}</span
      ><button class="delete-btn" @click="deleteTransactions(transaction.id)">x</button>
    </li>
  </ul>
</template>

<script setup>
import { defineProps } from "vue";

const emit = defineEmits(["transactionDeleted"]);
const props = defineProps({
  transactions: { type: Array, required: true },
});

const deleteTransactions = (id) => emit("transactionDeleted", id); 

</script>
