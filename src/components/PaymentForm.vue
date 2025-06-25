<template>
    <form @submit.prevent="submit" class="mb-6 space-y-2">
      <h2 class="font-semibold text-lg">Novo Pagamento</h2>
      <select v-model="form.loan" class="input">
        <option disabled value="">Selecione o Empréstimo</option>
        <option v-for="loan in loans" :value="loan.id" :key="loan.id">
          {{ loan.client }} - {{ loan.bank }}
        </option>
      </select>
      <input v-model="form.payment_date" type="date" class="input" />
      <input v-model.number="form.amount" type="number" placeholder="Valor" class="input" />
      <button type="submit" class="btn">Registrar Pagamento</button>
    </form>
  </template>
  
  <script setup>
  import { ref } from 'vue'
  import axios from 'axios'
  
  defineProps(['loans'])
  const emit = defineEmits(['payment-added'])
  
  const form = ref({
    loan: '',
    payment_date: '',
    amount: 0,
  })
  
  const submit = async () => {
    await axios.post('https://994a-200-236-253-115.ngrok-free.app/api/payment', form.value, {
    "headers": {
      'authorization': 'Token bd2b909d3ebc816bd107b336e0f85e847022a6c6',
      'ngrok-skip-browser-warning': 'true'
    }
  })
    form.value = { loan: '', payment_date: '', amount: 0 }
    emit('payment-added')
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
    background-color: #2196f3;
    color: white;
    padding: 0.5rem 1rem;
  }
  </style>
  