<script>
import axios from "axios";

export default {
  data() {
    return {
      city: "",
      error: "",
      info: null,
    };
  },
  computed: {
    cityName() {
      return "«" + this.city + "»";
    },
    showTemp() {
      return "Temperature: " + this.info.main.temp;
    },
    showFeelsLike() {
      return "Feels like: " + this.info.main.feels_like;
    },
    showMinTemp() {
      return "Min temperature: " + this.info.main.temp_min;
    },
    showMaxTemp() {
      return "Max temperature: " + this.info.main.temp_max;
    },
  },
  methods: {
    getWeather() {
      if (this.city.trim().length < 2) {
        this.error = "You have to write the real name of the city";
        return false;
      }

      this.error = "";

      axios
        .get(
          `https://api.openweathermap.org/data/2.5/weather?q=${this.city}&units=metric&appid=24126c32874529eeb7bdb8e2e8af75cc`
        )
        .then((result) => (this.info = result.data));
    },
  },
};
</script>

<template>
  <div class="wrapper">
    <h1>Weather App</h1>
    <p>See the weather in {{ city == "" ? "in your city" : cityName }}</p>

    <div class="input-group">
      <input v-model="city" type="text" placeholder="Enter the city" />

      <button
        @click="getWeather()"
        :disabled="city.trim().length < 2"
        class="button"
      >
        {{ city.trim().length < 2 ? "Enter the city" : "Get the weather" }}
      </button>
    </div>

    <p class="error">{{ error }}</p>

    <div v-if="info != null" class="weather-data">
      <p>{{ showTemp }}</p>
      <p>{{ showFeelsLike }}</p>
      <p>{{ showMinTemp }}</p>
      <p>{{ showMaxTemp }}</p>
    </div>
  </div>
</template>

<style scoped>
.wrapper {
  padding: 20px;
  width: 90%;
  max-width: 600px;
  min-height: 350px;
  margin: 50px auto;
  border-radius: 20px;
  background: linear-gradient(320deg, #29242e, #382a45);
  text-align: center;
  color: #fff;
  box-shadow: 0 4px 20px rgba(0, 0, 0, 0.5);
}

.wrapper h1 {
  margin-top: 20px;
  font-size: 2.2em;
}

.wrapper p {
  margin-top: 15px;
  font-size: 1.1em;
}

.weather-data p {
  margin-top: 10px;
  font-size: 1em;
  font-weight: 300;
}

.input-group {
  display: flex;
  justify-content: center;
  align-items: center;
  margin-top: 30px;
  margin-bottom: 20px;
}

.wrapper input {
  margin: 0;
  background: transparent;
  border: 0;
  border-bottom: 2px solid #110813;
  color: #fcfcfc;
  font-size: 16px;
  padding: 5px 8px;
  outline: none;
  width: 250px;
  flex-shrink: 0;
}

.button {
  background-color: #e3bc4b;
  color: #110813;
  font-weight: bold;
  border-radius: 10px;
  border: none;
  padding: 10px 15px;
  margin-left: 15px;
  cursor: pointer;
  transition: transform 300ms ease, background-color 300ms ease;
  flex-shrink: 0;
}

/* ------------------------------------- */
/* АДАПТАЦИЯ (МОБИЛЬНЫЙ) */
/* ------------------------------------- */
@media (max-width: 500px) {
  .input-group {
    flex-direction: column;
    margin-top: 20px;
  }

  .wrapper input {
    width: 90%;
    max-width: 100%;
    margin-bottom: 10px;
  }

  .button {
    width: 90%;
    margin-left: 0;
    margin-top: 10px;
  }
}
</style>
