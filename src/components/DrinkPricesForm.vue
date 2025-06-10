<script setup>
import {InputNumber} from "primevue"
import {Button} from "primevue"
import {onMounted, ref} from "vue"
const tabs = ref({
  Bill: 0,
  Jeremy: 0,
  Kirsten: 0,
  David: 0,
  Karleen: 0,
  Jenna: 0,
  Kim: 0,
})
const purchases = ref({
  Bill: [0,0],
  Jeremy: [0,0],
  Kirsten: [0,0],
  David: [0,0],
  Karleen: [0,0],
  Jenna: [0,0],
  Kim: [0,0],
})
const drinkPrices = ref({
  Bill: 4.5,
  Jeremy: 3,
  Kirsten: 5,
  David: 4,
  Karleen: 3.75,
  Jenna: 6.25,
  Kim: 5.5,
})
let result = ref('')
let rounds = ref(1)

const tallyRound = () => {
  let payor = ''
  for (const person in tabs.value) {
    tabs.value[person] += drinkPrices.value[person]
    if (payor == '' || tabs.value[person] > tabs.value[payor]) {
      payor = person
    }
  }
  return payor
}

const updateData = (payor) => {
  const bill = Object.values(drinkPrices.value).reduce((sum, x) => sum + x)
  tabs.value[payor] -= bill
  purchases.value[payor][0]++
  purchases.value[payor][1] += bill
  rounds.value++
  result = payor
}

const updateSession = () => {
  localStorage.setItem('tabs', JSON.stringify(tabs.value))
  localStorage.setItem('purchases', JSON.stringify(purchases.value))
  localStorage.setItem('drinkPrices', JSON.stringify(drinkPrices.value))
  localStorage.setItem('rounds', rounds.value)
}

const loadSession = () => {
  if(!localStorage.getItem('rounds')) return
  tabs.value = JSON.parse(localStorage.getItem('tabs'))
  purchases.value = JSON.parse(localStorage.getItem('purchases'))
  drinkPrices.value = JSON.parse(localStorage.getItem('drinkPrices'))
  rounds.value = parseInt(localStorage.getItem('rounds'))
}

const determineWhoPays = () => {
  const payor = tallyRound()
  updateData(payor)
  updateSession()
}

onMounted(() => {
  loadSession()
})
</script>

<template>
  <div class="flex">
    <div class="flex-1 font-bold">Team Member</div>
    <div class="flex-1 font-bold">Drink Price</div>
    <div class="flex-1 font-bold">Current Tab</div>
    <div class="flex-1 font-bold">Previously Paid</div>
  </div>
  <div>
    <div v-for="person in Object.keys(tabs)" class="flex">
      <div class="flex-1">
        <label :for="'tab-'+person" class="pt-3 right-0">
          {{ person }}
        </label>
      </div>
      <div class="flex-1">
        <InputNumber
          v-model="drinkPrices[person]"
          :inputId="'tab-'+person"
          :maxFractionDigits="2"
          :label="person"
        />
      </div>
      <div class="flex-1">
        <p>Current tab: ${{ Number(tabs[person]).toFixed(2) }}</p>
      </div>
      <div class="flex-1">
        <p>Paid {{ purchases[person][0] }} times for ${{ Number(purchases[person][1]).toFixed(2) }}</p>
      </div>
    </div>

    <Button
      :label="'Who buys round ' + rounds + '?'"
      class="mt-4 w-full"
      @click="determineWhoPays"
    />
    <h2 class="text-center">
      <span v-if="result">{{ result }} pays for round {{ rounds-1 }}</span>
      <span v-else>...</span>
    </h2>
  </div>
</template>