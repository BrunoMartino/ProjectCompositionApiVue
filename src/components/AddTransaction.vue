<template>
  <div>
    <h3 class="title2">Adicionar Nova transação:</h3>
    <form id="form" @submit.prevent="onSubmit">
      <div class="form-control">
        <label class="subtitle" for="text">Item da Transação</label>
        <input type="text" id="text" v-model="text" placeholder="Descreva a transação aqui" />
      </div>
      <div class="form-control">
        <label class="subtitle" for="amount">
          Quantia <br />
          ("-100,00" = Gasto ; "100,00" = Entrada)</label
        >
        <input type="text" id="amount" v-model="amount" placeholder="Coloque aqui o valor" />
      </div>
      <button class="btn">Adicionar Transação</button>
    </form>
  </div>
</template>

<script setup>
import { ref } from 'vue'
import { useToast } from 'vue-toastification'

const text = ref('')
const amount = ref('')

const emit = defineEmits(['transactionSubmitted'])

const toast = useToast()

const onSubmit = () => {
  if (!text.value || !amount.value) {
    toast.error('Ambos os Campos devem estar preenchidos')
    return
  } else if (isNaN(amount.value)) {
    toast.error('"Quantia" precisa de numeros positivos ou negativos')
  } else {
    const transactionData = {
      text: text.value,
      amount: parseFloat(amount.value)
    }

    emit('transactionSubmitted', transactionData)

    text.value = ''
    amount.value = ''
  }
}
</script>
