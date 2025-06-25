<template>
  <div class="overflow-x-auto mt-6">
    <table class="min-w-full bg-white border border-gray-200 rounded-xl shadow-md">
      <thead class="bg-blue-100 text-blue-800 uppercase text-sm font-semibold sticky top-0">
        <tr>
          <th class="px-4 py-3 text-left">Cliente</th>
          <th class="px-4 py-3 text-left">Banco</th>
          <th class="px-4 py-3 text-right">Valor Empréstimo</th>
          <th class="px-4 py-3 text-right">Pago</th>
          <th class="px-4 py-3 text-right">Saldo</th>
          <th class="px-4 py-3 text-right">Taxa de Juros</th>
          <th class="px-4 py-3 text-right">Data da solicitação</th>
          <th class="px-4 py-3 text-right">Endereço IP </th>
          <th class="px-4 py-3 text-center">Pagamentos</th>
        </tr>
      </thead>
      <tbody>
        <tr
          v-for="loan in loans"
          :key="loan.id"
          class="odd:bg-white even:bg-gray-50 hover:bg-gray-100 transition-colors"
        >
          <td class="px-4 py-3 font-medium text-gray-800">{{ loan.client }}</td>
          <td class="px-4 py-3 text-gray-700">{{ loan.bank }}</td>
          <td class="px-4 py-3 text-right text-gray-900">R$ {{ parseFloat(loan.amount).toFixed(2) }}</td>
          <td class="px-4 py-3 text-right text-green-600">R$ {{ parseFloat(loan.total_paid).toFixed(2) }}</td>
          <td class="px-4 py-3 text-right text-red-600">R$ {{ parseFloat(loan.outstanding_balance).toFixed(2) }}</td>
          <td class="px-4 py-3 text-right text-red-600">{{ loan.interest_rate }}%</td>
          <td class="px-4 py-3 text-right text-red-600">{{ formatDate(loan.requested_at) }}</td>
          <td class="px-4 py-3 text-right text-red-600">{{ loan.ip_address }}</td>
          <td class="px-4 py-3 text-center">
            <button
              @click="selectedLoan = loan"
              class="bg-blue-600 text-white text-sm px-3 py-1 rounded hover:bg-blue-700 transition"
            >
              Ver ({{ loan.payments.length }})
            </button>
          </td>
        </tr>
      </tbody>
    </table>

    <!-- Modal -->
    <div
      v-if="selectedLoan"
      class="fixed inset-0 bg-black/50 flex items-center justify-center z-50"
    >
      <div class="bg-white rounded-xl p-6 w-full max-w-md shadow-xl relative">
        <h2 class="text-lg font-semibold mb-4">
          Pagamentos de {{ selectedLoan.client }}
        </h2>
        <ul class="space-y-2 max-h-60 overflow-y-auto text-sm text-gray-800">
          <li
            v-for="p in selectedLoan.payments"
            :key="p.id"
            class="border-b pb-2"
          >
            📅 <strong>{{ p.payment_date }}</strong> — 💰 R$ {{ parseFloat(p.amount).toFixed(2) }}
          </li>
        </ul>
        <button
          @click="selectedLoan = null"
          class="absolute top-2 right-2 text-gray-500 hover:text-gray-700 text-xl"
          aria-label="Fechar"
        >
          ×
        </button>
        <div class="mt-4 text-right">
          <button
            @click="selectedLoan = null"
            class="bg-gray-200 text-gray-800 px-4 py-2 rounded hover:bg-gray-300"
          >
            Fechar
          </button>
        </div>
      </div>
    </div>
  </div>
</template>

<script setup>
import { ref } from 'vue'
defineProps(['loans'])

const formatDate = (isoString) => {
  const date = new Date(isoString)
  return date.toLocaleString('pt-BR', {
    day: '2-digit',
    month: '2-digit',
    year: 'numeric',
    hour: '2-digit',
    minute: '2-digit'
  })
}

const selectedLoan = ref(null)
</script>
