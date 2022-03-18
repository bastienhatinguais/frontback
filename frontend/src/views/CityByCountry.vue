<template>
  <div class="home">
    <h2>Villes par pays</h2>
    <div>
      <div class="form-group">
        <label for="country">Pays:</label>
        <select
          class="form-control"
          v-model="data.selectedCountry"
          @change="changeSelectedCountry($event)"
        >
          <option disabled value="0">Choisissez un pays</option>
          <option
            v-for="country in data.allCountries"
            :key="country.id"
            :value="country._links.self.href"
          >
            {{ country.name }}
          </option>
        </select>
      </div>
      <div class="mt-3">
        <city-list :cities="data.cities" />
      </div>
    </div>
  </div>
</template>

<script setup>
import { reactive, onMounted } from "vue";
// @ is an alias to /src
import CityList from "@/components/CityListWithoutCountry";

const data = reactive({
  cities: [],
  allCountries: [],
});

function changeSelectedCountry(event) {
  fetchCitiesOfCountry(event.target.value + "/cities");
}

function fetchCitiesOfCountry(url) {
  // Utilise l'API ad-hoc pour avoir le pays de chaque ville
  fetch(url)
    .then((response) => response.json())
    .then((json) => {
      data.cities = json._embedded.cities;
      console.log(data.cities);
    })
    .catch((error) => alert(error));
}

// Utilise l'API REST auto-générée pour avoir les pays
function fetchCountries() {
  fetch("api/countries")
    .then((response) => response.json())
    .then((json) => {
      data.allCountries = json._embedded.countries;
    })
    .catch((error) => alert(error));
}

// Au chargement du composant
onMounted(() => {
  fetchCountries(); // On récupère les pays (pour le sélecteur de pays)
});
</script>

