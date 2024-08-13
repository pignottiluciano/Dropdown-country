<template>
  <div>
    <h2>Select Your Country</h2>
    <CountrySelect v-model="countryselectnow" />
    <p>Selected Country: {{ countryselectnow?.countryName }}</p>
  </div>
</template>

<script lang="ts">
import { defineComponent, ref } from 'vue'
import CountrySelect from './dropdownCountries.vue'
import type { ContryType } from '../lib/countriesCode'

export default defineComponent({
  components: { CountrySelect },
  setup() {
    const countryselectnow = ref<ContryType | null>(null)

    return {
      countryselectnow
    }
  }
})
</script>

<style scoped></style>
