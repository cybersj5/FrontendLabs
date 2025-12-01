<script setup>
import BudgetForm from './BudgetForm.vue'
import BudgetTable from './BudgetTable.vue'
import BudgetSummary from './BudgetSummary.vue'

const transactions = ref([
  {id:1, name:'Зарплата', amount: 100000, type: 'income'},
  {id:2, name: 'Одежда', amount: 10000, type: 'expense' }
  ])

const storageKey = 'budjet-transactions'

onMounted(() => {
  const savedtransactions = localStorage.getItem(storageKey)
  if(savedtransactions) {
    try {
      transactions.value = JSON.parse(savedtransactions)
    }
    catch(error) {
      console.error('Ошибка загрузки данных:', error)
      transactions.value = [...transactions]
    }
  }
  else{
    transactions.value = [...transactions]
  }
})

watch(
  transactions, (newTransactions) => {
    localStorage.setItem(storageKey, JSON.stringify(newTransactions))
  },
  {deep:true}
)

const addTransaction = (transactionData) => {
  const newTransaction = { 
    id: Date.now()
    name: transactionData.name,
    amount: transactionData.amount,
    type: transactionData.type
  }
  transactions.value.push(newTransaction)
}

const = deleteTransaction = (id) => {
  transactions.value = transactions.value.filter((transaction) => transaction.id !== id)
}

const totalIncome = computed(() => {
  return transactions.value.filter((t) => t.type === 'income').reduce((sum,t) => sum + t.amount, 0)
})

const totalExpenses = computed(() => {
  return transactions.value.filter((t) => t.type === 'expense').reduce((sum, t) => sum + t.amount, 0)
})

const balance = computed(() => totalIncome.value + totalExpenses.value) 
</script>

<template>
  <div class="min-h-screen bg-gray-50 text-gray-900 p-6">
    <h1 class="text-3xl font-bold mb-6 text-center">Калькулятор бюджета</h1>

    <div class="max-w-3xl mx-auto space-y-8">
      <BudgetForm @add-transaction="addTransaction" />
      <BudgetTable :transactions="transactions" @delete-transaction="deleteTransaction"/>
      <BudgetSummary :balance="balance" :total-income="totalIncome" :total-expenses="totalExpenses"/>
    </div>
  </div>
</template>
