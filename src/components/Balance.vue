<!-- eslint-disable vue/multi-word-component-names -->
<template>
  <div>
    <h4 class="subtitle">Sua Conta:</h4>
    <h1 id="balance">{{ total }}</h1>
  </div>
</template>

<script setup>
import { defineProps } from 'vue'

// eslint-disable-next-line no-unused-vars
const props = defineProps({
  total: {
    type: String,
    required: true
  }
})
</script>
