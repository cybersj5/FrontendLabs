<script setup>
import { computed } from 'vue'

const props = defineProps({
  balance: {
    type: Number,
    required: true,
  },
  totalIncome: {
    type: Number,
    required: true,
  },
  totalExpenses: {
    type: Number,
    required: true,
  },
})

const isNegativeBalance = computed(() => props.balance < 0)
</script>

<template>
  <div class="bg-white shadow rounded-xl p-4 text-center">
    <h2 class="text-lg font-semibold mb-3">Сводка</h2>

    <div class="grid grid-cols-3 gap-4">
      <div>
        <p class="text-gray-500">Доходы</p>
        <p class="text-green-600 text-xl font-semibold">+{{ totalIncome }} руб.</p>
      </div>
      <div>
        <p class="text-gray-500">Расходы</p>
        <p class="text-red-600 text-xl font-semibold">{{ totalExpenses }} руб.</p>
      </div>
      <div>
        <p class="text-gray-500">Баланс</p>
        <p class="text-gray-800 text-xl font-semibold">{{ balance }} руб.</p>
      </div>
    </div>
    <p
      v-if="isNegativeBalance"
      class="mt-4 p-3 bg-red-50 border border-red-200 text-red-700 rounded-lg font-medium text-center"
    >
      Ваш баланс отрицательный!
    </p>
    <p
      v-else
      class="mt-4 p-3 bg-green-50 border border-green-200 text-green-700 rounded-lg font-medium text-center"
    >
      Ваш бюджет в порядке!
    </p>
  </div>
</template>

