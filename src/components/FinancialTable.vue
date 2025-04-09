<template>
  <div class="max-w-2xl mx-auto p-4 bg-white rounded-2xl shadow">
    <h2 class="text-xl font-bold mb-4 text-center">Controle Financeiro</h2>
    
    <form @submit.prevent="addTransaction" class="space-y-4">
      <div class="grid grid-cols-2 gap-4">
        <input v-model="form.description" type="text" placeholder="Descrição (ex: mercado)" class="border p-2 rounded w-full" required />
        <input v-model.number="form.amount" type="number" step="0.01" placeholder="Valor (ex: 100.50)" class="border p-2 rounded w-full" required />
      </div>

      <select v-model="form.type" class="w-full border p-2 rounded" required>
        <option value="">Tipo</option>
        <option value="entrada">Entrada</option>
        <option value="saida">Saída</option>
      </select>

      <button type="submit" class="bg-blue-500 hover:bg-blue-600 text-white px-4 py-2 rounded w-full">
        Adicionar Transação
      </button>
    </form>

    <table class="w-full mt-6 text-left border-t">
      <thead>
        <tr>
          <th class="py-2">Descrição</th>
          <th class="py-2">Valor</th>
          <th class="py-2">Tipo</th>
        </tr>
      </thead>
      <tbody>
        <tr v-for="(transaction, index) in transactions" :key="index" class="border-t">
          <td class="py-2">{{ transaction.description }}</td>
          <td :class="transaction.type === 'entrada' ? 'text-green-600' : 'text-red-600'" class="py-2">
            R$ {{ transaction.amount.toFixed(2) }}
          </td>
          <td class="py-2 capitalize">{{ transaction.type }}</td>
        </tr>
      </tbody>
    </table>
  </div>
</template>

<script lang="ts" setup>
import { reactive, ref } from 'vue'

interface Transaction {
  description: string
  amount: number
  type: 'entrada' | 'saida'
}

const transactions = ref<Transaction[]>([])

const form = reactive<Transaction>({
  description: '',
  amount: 0,
  type: 'entrada',
})

function addTransaction() {
  if (form.description && form.amount && form.type) {
    transactions.value.push({ ...form })
    form.description = ''
    form.amount = 0
    form.type = 'entrada'
  }
}
</script>
