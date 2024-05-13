<template>
  <div class="weather" :class="weatherClass">
    <div class="container">
      <div class="card weather-form">
        <input
          type="text"
          class="weather-form__input"
          v-model="SearchQuery"
          @keyup.enter="weatherSearch"
          placeholder="Enter city"
        />
        <button class="weather-form__btn" @click="weatherSearch">Search</button>
      </div>
      <div class="card weather-load" v-if="loading">Loading...</div>
      <div
        class="weather-info"
        v-show="
          !error &&
          location &&
          temperature !== null &&
          temperature !== undefined &&
          description
        "
      >
        <div class="card" v-if="error">Error</div>
        <div class="weather-info__text">
          <p class="card">{{ location }}</p>
          <p class="card">{{ temperature }}°C</p>
          <p class="card">{{ description }}</p>
        </div>
      </div>
    </div>
    <div class="weather-bg" :class="weatherClass">
      <div>
        <img
          src="./assets/bg.jpg"
          class="weather-bg__img bg"
          alt="Background"
        />
        <img
          src="./assets/outcast.jpg"
          class="weather-bg__img overcast"
          alt="Rain"
        />
        <img
          src="./assets/party-cloudy.jpg"
          class="weather-bg__img partly-cloudy"
          alt="Cloudy"
        />
        <img
          src="./assets/sunny.jpg"
          class="weather-bg__img sunny"
          alt="Sunny"
        />
      </div>
    </div>
  </div>
</template>

<script>
export default {
  data() {
    return {
      location: "",
      temperature: 0,
      description: "",
      loading: false,
      error: false,
      SearchQuery: "",
    };
  },
  computed: {
    weatherClass() {
      if (this.description.includes("Sunny")) {
        return "sunny";
      } else if (this.description.includes("Overcast")) {
        return "overcast";
      } else if (this.description.includes("Partly cloudy")) {
        return "partly-cloudy";
      } else {
        return "";
      }
    },
  },
  methods: {
    weatherSearch() {
      this.loading = true;
      this.error = false;
      fetch(
        `http://api.weatherapi.com/v1/current.json?key=907872a9fa6c4cd084e10939241305&q=${this.SearchQuery}`
      )
        .then((response) => response.json())
        .then((data) => {
          this.loading = false;
          this.location = data.location.name;
          this.temperature = data.current.temp_c;
          this.description = data.current.condition.text;
          this.resetSearchQuery();
        })
        .catch((error) => {
          this.loading = false;
          this.error = true;
          console.error(error);
        });
    },
    resetSearchQuery() {
      this.SearchQuery = "";
    },
  },
};
</script>
