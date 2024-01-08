<template>
  <div id="app">
    <HeaderComp />
    <div class="container mx-auto">
      <BalanceComp :total="total" />
      <IncomeExpense :income="income" :expenses="expenses" />
      <TransactionList :transactions="transactions" @transactionDeleted="handleDelete" />
      <AddTransaction @transactionSubmitted="handleTransactions" />
    </div>
  </div>
</template>

<script setup>
import HeaderComp from '@/components/Header.vue'
import BalanceComp from '@/components/Balance.vue'
import IncomeExpense from '@/components/IncomeExpense.vue'
import TransactionList from '@/components/TransactionList.vue'
import AddTransaction from '@/components/AddTransaction.vue'

import { ref, computed, onMounted } from 'vue'
import { useToast } from 'vue-toastification'

const toast = useToast()

const transactions = ref([])

onMounted(() => {
  const savedTransactions = JSON.parse(localStorage.getItem('transactions'))

  if (savedTransactions) {
    transactions.value = savedTransactions
  }
})

//Get total
const total = computed(() => {
  const calculatedTotal = transactions.value.reduce((acc, transaction) => {
    return acc + transaction.amount
  }, 0)

  return calculatedTotal.toLocaleString('pt-br', {
    style: 'currency',
    currency: 'BRL'
  })
})

//Get income
const income = computed(() => {
  const calculatedIncome = transactions.value
    .filter((transaction) => transaction.amount > 0)
    .reduce((acc, transaction) => {
      return acc + transaction.amount
    }, 0)

  return calculatedIncome.toLocaleString('pt-br', {
    style: 'currency',
    currency: 'BRL'
  })
})
//Get expenses
const expenses = computed(() => {
  const calculatedExpenses = transactions.value
    .filter((transaction) => transaction.amount < 0)
    .reduce((acc, transaction) => {
      return acc + transaction.amount
    }, 0)

  return calculatedExpenses.toLocaleString('pt-br', {
    style: 'currency',
    currency: 'BRL'
  })
})

//Add Transaction

const handleTransactions = (transactionData) => {
  transactions.value.push({
    id: generateUniqueId(),
    text: transactionData.text,
    amount: transactionData.amount
  })

  saveTransactionsToLocalStorage()

  toast.success('Nova Transação Adicionada')
}

//Generate Unique Id

const generateUniqueId = () => {
  return Math.floor(Math.random() * 10000000)
}

//Delete Transaction
const handleDelete = (id) => {
  transactions.value = transactions.value.filter((transaction) => transaction.id !== id)
  saveTransactionsToLocalStorage()
  toast.info('Transação Deletada')
}

//Save to LocalStorage
const saveTransactionsToLocalStorage = () => {
  localStorage.setItem('transactions', JSON.stringify(transactions.value))
}
</script>

<style></style>
