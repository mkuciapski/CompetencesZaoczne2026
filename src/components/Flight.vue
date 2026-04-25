<script setup lang="ts">
import { ref, computed, watch, watchEffect } from 'vue'
const _props = defineProps<{ defaultTicketNumber: number; visible: boolean; flight: { ID: number; City: string; Tickets: number } }>()

const _emit = defineEmits<{
  update: [flight: { ID: number; City: string; Tickets: number }]
}>()

const _flight = ref<{ ID: number; City: string; Tickets: number }>({
  ID: _props.flight.ID,
  City: _props.flight.City,
  Tickets: _props.flight.Tickets,
})

watch(
  () => _props.flight,
  () => {
    _flight.value.City = _props.flight.City
    _flight.value.Tickets = _props.flight.Tickets
  },
  { deep: true },
)

function update() {
  _emit('update', _flight.value)
}

const _message = ref('')
//#region OLD
// watch(
//   () => _props.visible,
//   (current, previous) => {
//     if (!previous && current) {
//       console.log(`${previous}:${current}`)
//       _flight.value = { City: '', Tickets: _props.defaultTicketNumber ?? 1 }
//     }
//   },
// )

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
//#endregion OLD

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
    <button @click="update">Save</button>
    <p class="text-red-700">{{ _message }}</p>
  </div>
</template>
<style scoped></style>
