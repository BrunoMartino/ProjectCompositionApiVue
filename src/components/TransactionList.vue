<!-- eslint-disable no-unused-vars -->
<template>
  <div>
    <h3 class="title2">Histórico</h3>
    <ul id="list" class="list">
      <li
        v-for="transaction in transactions"
        :key="transaction.id"
        :class="transaction.amount < 0 ? 'minus' : 'plus'"
      >
        {{ transaction.text }} <span>R${{ transaction.amount }}</span
        ><button @click="deleteTransaction(transaction.id)" class="delete-btn">X</button>
      </li>
    </ul>
  </div>
</template>

<script setup>
import { defineProps } from 'vue'

const emit = defineEmits(['transactionDeleted'])

// eslint-disable-next-line no-unused-vars
const props = defineProps({
  transactions: {
    type: Array,
    required: true
  }
})

const deleteTransaction = (id) => {
  emit('transactionDeleted', id)
}
</script>
