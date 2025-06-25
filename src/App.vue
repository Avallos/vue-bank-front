<template>
  <div class="p-6">
    <h1 class="text-2xl font-bold mb-4">Sistema de Empréstimos</h1>
    <LoanTable :loans="loans" />
    <LoanForm @loan-added="fetchLoans" />
    <PaymentForm :loans="loans" @payment-added="fetchLoans" />
  </div>
</template>

<script setup>
import { ref, onMounted } from 'vue'
import axios from 'axios'
import LoanTable from './components/LoanTable.vue'
import LoanForm from './components/LoanForm.vue'
import PaymentForm from './components/PaymentForm.vue'

axios.defaults.withCredentials = false
const loans = ref([])

const fetchLoans = async () => {
  const res = await axios.get("https://994a-200-236-253-115.ngrok-free.app/api/loan", {
        "headers": {
          "authorization": "Token bd2b909d3ebc816bd107b336e0f85e847022a6c6",
          "ngrok-skip-browser-warning": 'true'
        }
      })
  loans.value = res.data
}

onMounted(fetchLoans)
</script>
