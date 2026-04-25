<script setup lang="ts">
import { ref, computed, watch, watchEffect } from 'vue'
const _props = defineProps<{ defaultTicketNumber: number; visible: boolean }>()

const _flight = ref({
  City: '',
  Tickets: _props.defaultTicketNumber ?? 1,
})

watch(
  () => _props.visible,
  (current, previous) => {
    if (!previous && current) {
      console.log(`${previous}:${current}`)
      _flight.value = { City: '', Tickets: _props.defaultTicketNumber ?? 1 }
    }
  },
)

const _message = ref('')
/*watch(
  () => _flight.value,
  (current, previous) => {
    const dangerousCities = ['Beirut', 'Sopot']

    if (current.Tickets < 1) _message.value = 'Proszę prowadzić liczbę biletów'
    else if (dangerousCities.includes(current.City)) _message.value = 'Nie jedź do tego miasta'
    else _message.value = ''
  },
  { deep: true, immediate: true },
)*/
watchEffect(() => {
  const dangerousCities = ['Beirut', 'Sopot']

  if (_flight.value.Tickets < 1) _message.value = 'Proszę prowadzić liczbę biletów'
  else if (dangerousCities.includes(_flight.value.City)) _message.value = 'Nie jedź do tego miasta'
  else _message.value = ''
})
</script>
<template>
  <div class="border-1 rounded-lg">
    <input placeholder="lokalizacja" v-model="_flight.City" />
    <input min="0" step="1" v-model="_flight.Tickets" type="number" />
    <p class="text-red-700">{{ _message }}</p>
  </div>
</template>
<style scoped></style>
