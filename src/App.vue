<script setup>
import DrinkPricesForm from "@/components/DrinkPricesForm.vue";
import {Button} from "primevue";
import {ref} from "vue";


const key = ref(0)
const clearSession = () => {
  sessionStorage.clear()
  key.value++
}
</script>

<template>
  <div>
    <header class="pb-8 pt-8">
      <img alt="Bertram Labs" class="logo" src="./assets/logo.svg" />
    </header>

    <main>
      <Button
          class="absolute"
          label="Reset"
          severity="danger"
          @click="clearSession"
      />
      <div class="m-auto text-center mb-4">
        <h2>Coffee Tabulator</h2>
        <h3>Submission by Cameron West</h3>
        <p>Feel free to update the pricing between rounds</p>
      </div>

      <DrinkPricesForm :key="key"/>
    </main>
  </div>
</template>

<style scoped>

header {
  line-height: 1.5;
}

.logo {
  display: block;
  margin: 0 auto 2rem;
}

@media (min-width: 1024px) {
  header {
    display: flex;
    place-items: center;
    padding-right: calc(var(--section-gap) / 2);
  }

  .logo {
    margin: 0 2rem 0 0;
  }

  body {
    background: #fff;
  }
}
</style>
