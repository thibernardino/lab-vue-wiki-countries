<template>
  <!-- Using an object to store all data properties and use it on template -->
  <div>
    <img
      v-bind:src="`https://flagcdn.com/w320/${alpha2Code.toLowerCase()}.png`"
      alt=""
      class="mb-5"
    />
    <h1>{{ countryInfo.name.common }}</h1>
    <ul class="list-group list-group-flush">
      <li
        class="list-group-item d-flex justify-content-between align-items-center"
      >
        <p class="fw-bold">Capital</p>
        <p class="me-5">{{ countryInfo.capital[0] }}</p>
      </li>
      <li
        class="list-group-item d-flex justify-content-between align-items-center"
      >
        <p class="fw-bold">Area</p>
        <p class="me-5">{{ countryInfo.area }} km2</p>
      </li>
      <li class="list-group-item">
        <p class="fw-bold">Borders:</p>
        <p v-if="countryInfo.borders.length === 0">
          This country has no borders <br />
        </p>
        <p v-else v-for="(border, index) in countryInfo.borders" :key="index">
          <router-link :to="`/list/${border}`">{{ border }}</router-link>
        </p>
      </li>
    </ul>
  </div>
</template>

<script>
export default {
  name: "CountriesDetails",

  data() {
    return {
      name: "",
      capital: "",
      alpha3Code: "",
      area: "",
      borders: [],
      alpha2Code: "",

      // Pass data from API on an object
      countryInfo: {},
    };
  },

  methods: {
    async getCountryByAlphaCode() {
      // Apuntamos con el thos$route.params a el valor de la url que queremos sacar, debido a que necesitamos el countryCode debemos guardar el valor de ese codigo deentro de un formato de dato data() para poder usarlo.
      this.alpha3Code = this.$route.params.alpha3Code;
      const response = await fetch(
        `https://ih-countries-api.herokuapp.com/countries/${this.alpha3Code}`
      );

      const finalResponse = await response.json();

      console.log(finalResponse);

      this.name = finalResponse.name.common;
      this.capital = finalResponse.capital[0];
      this.area = finalResponse.area;
      this.borders = finalResponse.borders;
      this.alpha2Code = finalResponse.alpha2Code;

      this.countryInfo = finalResponse;
    },
  },
  mounted() {
    this.getCountryByAlphaCode();
  },
  computed: {
    countryCode() {
      return this.$route.params.alpha3Code;
    },
  },

  watch: {
    countryCode(newCountryCode) {
      this.getCountryByAlphaCode();
    },
  },
};
</script>

<style></style>
