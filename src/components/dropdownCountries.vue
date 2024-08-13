<template>
  <div>
    <label for="country-select">Select a Country:</label>
    <div class="dropdown" @click="toggleDropdown">
      <div class="dropdown-selected">
        <img
          v-if="internalValue"
          :src="internalValue.image"
          :alt="internalValue.countryName"
          class="flag-icon"
        />
        {{ internalValue ? internalValue.countryName : 'Choose a country' }}
      </div>
      <ul v-show="dropdownOpen" class="dropdown-list">
        <li
          v-for="country in countries"
          :key="country.value"
          @click="selectCountry(country, $event)"
        >
          <img :src="country.image" :alt="country.countryName" class="flag-icon" />
          {{ country.countryName }}
        </li>
      </ul>
    </div>
  </div>
</template>

<script lang="ts">
import { defineComponent, ref, type PropType, watch } from 'vue'
import { countriesCode, type ContryType } from '../lib/countriesCode'

export default defineComponent({
  name: 'CountrySelect',
  props: {
    modelValue: {
      type: Object as PropType<ContryType | null>,
      default: null
    }
  },
  emits: ['update:modelValue'],
  setup(props, { emit }) {
    const countries = ref<Array<ContryType>>(countriesCode())
    const internalValue = ref<ContryType | null>(props.modelValue)
    const dropdownOpen = ref<boolean>(false)

    const toggleDropdown = () => {
      dropdownOpen.value = !dropdownOpen.value
    }

    const selectCountry = (country: ContryType, event: MouseEvent) => {
      event.stopPropagation()
      internalValue.value = country
      dropdownOpen.value = false
      emit('update:modelValue', country)
    }

    watch(
      () => props.modelValue,
      (newVal) => {
        internalValue.value = newVal
      },
      { immediate: true }
    )

    return {
      countries,
      internalValue,
      dropdownOpen,
      toggleDropdown,
      selectCountry
    }
  }
})
</script>

<style scoped lang="scss">
.dropdown {
  position: relative;
  display: inline-block;
  cursor: pointer;
  min-width: 150px;
}

.dropdown-selected {
  padding: 10px;
  border: 1px solid #ccc;
  display: flex;
  align-items: center;
}

.dropdown-list {
  position: absolute;
  top: 100%;
  left: 0;
  right: 0;
  border: 1px solid black;
  background-color: gray;
  max-height: 500px;
  width: 100%;
  min-width: 170px;
  overflow-y: auto;
  z-index: 1000;
  padding-left: 0px;
}

.dropdown-list li {
  padding: 10px;
  display: flex;
  align-items: center;
}

.dropdown-list li:hover {
  background-color: #f0f0f0;
}

.flag-icon {
  width: 20px;
  height: 15px;
  margin-right: 8px;
}
</style>
