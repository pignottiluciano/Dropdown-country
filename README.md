
# Dropdown Country

## Description

This project is a Vue.js application built with TypeScript. The application features a customizable country selection dropdown, allowing users to choose a country from a list, including the country’s name, phone code, and flag.

## Features

- Customizable country selection dropdown.
- Displays country names and flags.
- Built with Vue.js and TypeScript.
- Fully reusable component for use in other parts of the application.

## Installation

To install and run this project locally:

1. Clone the repository:
   ```bash
   git clone https://github.com/pignottiluciano/dropdown-country.git
   ```

2. Navigate to the project directory:
   ```bash
   cd dropdown-country
   ```

3. Install dependencies:
   ```bash
   npm install
   ```

4. Run the development server:
   ```bash
   npm run serve
   ```

## Usage

Include the `CountrySelect` component in any Vue component to enable the country selection feature.

```vue
<template>
  <div>
    <CountrySelect v-model="selectedCountry" />
  </div>
</template>

<script>
import { defineComponent, ref } from 'vue';
import CountrySelect from './components/CountrySelect.vue';

export default defineComponent({
  components: { CountrySelect },
  setup() {
    const selectedCountry = ref(null);

    return { selectedCountry };
  },
});
</script>
```
