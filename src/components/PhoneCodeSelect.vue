<template>
  <section class="contries-section">
    <h2>Select Your Phone Code</h2>
    <CountrySelect
      :valueCountry="phoneCodeselectnow"
      :isPhoneCode="true"
      v-model="phoneCodeselectnow"
    />
    <p>Selected Phone Code: {{ phoneCodeselectnow }}</p>
  </section>
</template>

<script lang="ts">
import { defineComponent, ref } from 'vue'
import CountrySelect from './dropdownCountries.vue'

export default defineComponent({
  components: { CountrySelect },
  setup() {
    const phoneCodeselectnow = ref<string>('')

    return {
      phoneCodeselectnow
    }
  }
})
</script>

<style scoped lang="scss">
.contries-section {
  height: 100%;
  width: 100%;
  padding: 24px;
}
</style>
