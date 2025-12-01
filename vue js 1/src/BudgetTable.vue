<script setup>
import { ref, computed } from 'vue'

const props = defineProps({
  transactions: {
    type: Array,
    required: true,
  },
})

const emit = defineEmits({ 'delete-transaction': null })

const sortField = ref('amount')
const sortDirection = ref('asc')

const deleteTransaction = (id) => emit('delete-transaction', id)

const sortedTransactions = computed(() => {
  const sorted = [...props.transactions]

  sorted.sort((a, b) => {
    return sortDirection.value === 'asc' ? a.amount - b.amount : b.amount - a.amount
  })

  return sorted
})

const toggleSort = (field) => {
  if (sortField.value === field) {
    sortDirection.value = sortDirection.value === 'asc' ? 'desc' : 'asc'
  } else {
    sortField.value = field
    sortDirection.value = 'asc'
  }
}

const getSortIcon = () => {
  return sortDirection.value === 'asc' ? '↑' : '↓'
}
</script>

<template>
  <div class="bg-white shadow rounded-xl p-4">
    <h2 class="text-lg font-semibold mb-3">Операции</h2>

    <table class="w-full text-left border-collapse">
      <thead>
        <tr class="border-b">
          <th class="p-2">Название</th>
          <th class="p-2">Тип</th>
          <th class="p-2 cursor-pointer hover:bg-gray-100" @click="toggleSort('amount')">
            Сумма {{ getSortIcon() }}
          </th>
          <th class="p-2">Действия</th>
        </tr>
      </thead>
      <tbody>
        <tr
          v-for="transaction in sortedTransactions"
          :key="transaction.id"
          class="border-b hover:bg-gray-50"
        >
          <td class="p-2">{{ transaction.name }}</td>
          <td
            class="p-2 font-medium"
            :class="transaction.type === 'income' ? 'text-green-600' : 'text-red-600'"
          >
            {{ transaction.type === 'income' ? 'Доход' : 'Расход' }}
          </td>
          <td
            class="p-2 font-medium"
            :class="transaction.type === 'income' ? 'text-green-600' : 'text-red-600'"
          >
            {{ transaction.type === 'income' ? '+' : '' }}{{ transaction.amount }}
          </td>
          <td class="p-2">
            <button
              @click="deleteTransaction(transaction.id)"
              class="text-sm bg-red-500 text-white px-3 py-1 rounded hover:bg-red-600 transition"
            >
              Удалить
            </button>
          </td>
        </tr>
        <tr v-if="transactions.length === 0">
          <td colspan="4" class="p-4 text-center text-gray-500">Нет операций</td>
        </tr>
      </tbody>
    </table>
  </div>
</template>
