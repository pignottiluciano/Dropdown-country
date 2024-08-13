<template>
  <div>
    <label v-if="!isPhoneCode" for="country-select">Select a Country:</label>
    <label v-if="isPhoneCode" for="country-select">Select a Phone Code:</label>
    <div class="dropdown" @click="toggleDropdown">
      <div class="dropdown-selected">
        <img
          v-if="internalValue"
          :src="internalValue.image"
          :alt="internalValue.countryName"
          class="flag-icon"
        />
        <span v-if="!isPhoneCode">
          {{ internalValue ? internalValue.countryName : 'Choose a country' }}
        </span>
        <span v-if="isPhoneCode">
          {{ internalValue ? internalValue.phoneCode : 'Choose a country' }}
        </span>
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
import { defineComponent, ref, watch } from 'vue'
import { countriesCode, type CountryType } from '../lib/countriesCode'

export default defineComponent({
  name: 'CountrySelect',
  props: {
    valueCountry: {
      type: String,
      required: true
    },
    isPhoneCode: Boolean
  },
  emits: ['update:modelValue'],
  setup(props, { emit }) {
    const countries = ref<Array<CountryType>>(countriesCode())
    const internalValue = ref<CountryType | null>()
    const dropdownOpen = ref<boolean>(false)

    const toggleDropdown = () => {
      dropdownOpen.value = !dropdownOpen.value
    }

    const selectCountry = (country: CountryType, event: MouseEvent) => {
      event.stopPropagation()
      internalValue.value = country
      dropdownOpen.value = false
      if (props.isPhoneCode) {
        emit('update:modelValue', country.phoneCode)
      } else {
        emit('update:modelValue', country.value)
      }
    }

    watch(
      () => props.valueCountry,
      (newVal: string) => {
        let selectedCountry = null
        if (props.isPhoneCode) {
          selectedCountry = countries.value.find(
            (country: CountryType) => country.phoneCode === newVal
          )
        } else {
          selectedCountry = countries.value.find((country: CountryType) => country.value === newVal)
        }

        if (selectedCountry) {
          internalValue.value = selectedCountry
        }
      },
      {
        immediate: true
      }
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
  width: 100%;
  text-align: left;
  color: #697077;
  font-family: 'Filson Pro', sans-serif;
  font-size: 0.71vw;
  font-weight: 400;
  margin: 8px;
  margin-bottom: 0;
  border: none;
  outline: none;
  border-bottom: 1px solid #c1c7cd;
  transition: 0.4s ease-in-out;
  text-transform: capitalize;
  cursor: pointer;
  height: 40px;
}

.dropdown-selected {
  padding: 0;
  width: 100%;
  height: 100%;
  display: flex;
  align-items: center;
  padding-left: 20px;
}
span {
  display: flex;
  width: 100%;
  height: 100%;
  align-items: center;
  /* justify-content: flex-start; */
  /* gap: 8px; */
  font-size: 18px;
}

.dropdown-list {
  position: absolute;
  display: flex;
  flex-direction: column;
  padding: 16px;
  border: 1px solid rgba(68, 66, 63, 0.1);
  background: #f6f6f6;
  border-radius: 0 0 4px 4px;
  width: 100%;
  max-height: 250px;
  display: flex;
  flex-direction: column;
  max-height: 248px;
  overflow-x: hidden;
  overflow-y: auto;
}

.dropdown-list li {
  padding: 4px;
  display: flex;
  align-items: center;
  font-size: 14px;
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
