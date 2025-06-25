<template>
    <form @submit.prevent="submit" class="mb-4 space-y-2">
      <h2 class="font-semibold text-lg">Novo Empréstimo</h2>
      <input v-model="form.client" placeholder="Cliente" class="input" />
      <input v-model="form.bank" placeholder="Banco" class="input" />
      <input v-model.number="form.amount" type="number" placeholder="Valor" class="input" />
      <input v-model.number="form.interest_rate" type="number" placeholder="Juros (%)" class="input" />
      <button type="submit" class="btn">Criar</button>
    </form>
  </template>
  
  <script setup>
  import { ref } from 'vue'
  import axios from 'axios'
  
  const emit = defineEmits(['loan-added'])
  
  const form = ref({
    client: '',
    bank: '',
    amount: 0,
    interest_rate: 0,
  })
  
  const submit = async () => {
    await axios.post('https://994a-200-236-253-115.ngrok-free.app/api/loan', form.value, {
    "headers": {
      'authorization': 'Token bd2b909d3ebc816bd107b336e0f85e847022a6c6',
      'ngrok-skip-browser-warning': 'true'
    }
  })
    form.value = { client: '', bank: '', amount: 0, interest_rate: 0 }
    emit('loan-added')
  }
  </script>
  
  <style scoped>
  .input {
    display: block;
    padding: 0.5rem;
    border: 1px solid #ccc;
    width: 100%;
  }
  .btn {
    background-color: #4caf50;
    color: white;
    padding: 0.5rem 1rem;
  }
  </style>
  