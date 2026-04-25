<script setup lang="ts">
import { ref, computed, watch, watchEffect } from 'vue'
import { users } from '@/assets/users'
import Flight from '@/components/Flight.vue'

// #region OLD
const _users = ref(users)
const _user = ref({ Name: '<b>John Doe</b>', Age: 30 })

setTimeout(() => {
  _user.value = { Name: '<b>Michał K</b>', Age: 30 }
  // _users.value = users.filter((u) => u.Age > 40)
  _users.value.push({ Name: '<b>Donald Kowalski</b>', Age: 60 })
  _users.value[0].Age = 70
}, 5000)

const _fontSize = ref('12px')
setTimeout(() => {
  _fontSize.value = new Date().getSeconds() + 'px'
}, 5000)

const _agedUsers = computed((previous) => {
  console.warn('previous aged users', previous)
  return _users.value.filter((user) => user.Age > 50)
})
// #endregion OLD

const _flights = ref([
  { ID: 1, City: 'Ankara', Tickets: 4 },
  { ID: 2, City: 'Ateny', Tickets: 3 },
  { ID: 3, City: 'Barcelona', Tickets: 1 },
  { ID: 4, City: 'Azory', Tickets: 3 },
])
const _showFlightPanel = ref(false)

function updateFlight(flight: { ID: number; City: string; Tickets: number }) {
  const flightToUpdate = _flights.value.find((flightInList) => flightInList.ID === flight.ID)

  if (flightToUpdate) {
    flightToUpdate.City = flight.City
    flightToUpdate.Tickets = flight.Tickets
  }
}
</script>

<template>
  <input type="checkbox" v-model="_showFlightPanel" />
  <Flight v-for="flight in _flights" :key="flight.City" class="my-2" v-show="_showFlightPanel" :default-ticket-number="2" :visible="_showFlightPanel" :flight="flight" @update="updateFlight" />

  <pre>
    {{ _flights }}
  </pre>

  <!-- <div>
    <h1>TEMP</h1>
    <p class="user-name">{{ _user.Name }}</p>
    <p v-html="_user.Name"></p>

    <input v-model.lazy.trim="_user.Name" />
    <input v-model="_user.Name" />

    <pre>{{ _agedUsers }}</pre>

    <div style="margin-top: 20px">
      <input type="submit" />
    </div>
  </div> -->
</template>
<style scoped>
.user-name {
  font-size: v-bind('_fontSize');
}
</style>
