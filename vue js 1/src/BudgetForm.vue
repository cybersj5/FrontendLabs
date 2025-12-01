<script setup>
import { ref } from 'vue'

const emit = defineEmits(['add-transaction'])

const formData = ref({
  name: '',
  amount: 0,
  type: 'income',
})

const submitForm = () => {
  if (!formData.value.name.trim() || formData.value.amount <= 0) {
    alert('Заполните все поля корректно ')
    return
  }

  const amount =
    formData.value.type === 'income'
      ? Number(formData.value.amount)
      : -Number(formData.value.amount)

  emit('add-transaction', {
    name: formData.value.name,
    amount: Number(amount),
    type: formData.value.type,
  })

  formData.value.name = ''
  formData.value.amount = 0
  formData.value.type = 'income'
}
</script>

<template>
  <div class="bg-white shadow rounded-xl p-4 flex flex-row gap-4">
    <input v-model="formData.name" type="text" placeholder="Название" class="border p-2 flex-1" />

    <input v-model="formData.amount" type="number" placeholder="Сумма" class="border p-2 w-32" />

    <select v-model="formData.type" class="border p-2 w-32">
      <option value="income">Доход</option>
      <option value="expense">Расход</option>
    </select>

    <button
      @click="submitForm"
      type="button"
      class="bg-blue-600 text-white px-4 py-2 hover:bg-blue-700 transition"
    >
      Добавить
    </button>
  </div>
</template>

<style scoped>
::v-deep input::-webkit-outer-spin-button,
::v-deep input::-webkit-inner-spin-button {
  -webkit-appearance: none;
  margin: 0;
}
</style>
