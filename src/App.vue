<template>
  <q-layout view="hHh lpR fFf">
    <q-header elevated class="text-white" style="background-color: #4B0082;">
      <q-toolbar>
        <q-toolbar-title class="text-center">
          <h3 style="font-family: Calibri, 'Segoe UI', Arial, sans-serif; color: white; font-weight: 700;">Realtime Weather Widget</h3>
        </q-toolbar-title>
        
        <!-- Dropdown menu for tasks 1-7 -->
        <q-btn-dropdown
          flat
          dense
          color="white"
          icon="menu"
          label="SINDI TASKS"
          class="q-ml-auto"
          style="color: white;"
        >
          <q-list>
            <q-item v-for="(task, index) in taskLinks" :key="index" clickable v-ripple @click="goToLink(task.link)">
              <q-item-section>
                <q-item-label>{{ task.name }}</q-item-label>
              </q-item-section>
            </q-item>
          </q-list>
        </q-btn-dropdown>

      </q-toolbar>
    </q-header>

    <q-page-container>
      <div class="weather">
        <h1 class="weather-title"></h1>
        <form @submit.prevent="fetchWeather">
          <input v-model="city" placeholder="Enter city or location" required style="font-size: 1.5rem; padding: 6px; font-family: Calibri, 'Segoe UI', Arial, sans-serif;" />
          <button type="submit" style="font-size: 1.5rem; padding: 6px;">Fetch Weather</button>
        </form>
        <div v-if="loading" style="color: white; font-family: Calibri, 'Segoe UI', Arial, sans-serif;">Loading...</div>
        <div v-if="error" class="error" style="color: red; font-family: Calibri, 'Segoe UI', Arial, sans-serif;">{{ error }}</div>
        
        <!-- Weather Data Box -->
        <div v-if="weather && !error" class="weather-box">
          <h4 class="weather-data">{{ weather.name }}</h4>
          <p class="weather-data">Temperature: {{ (weather.main.temp - 273.15).toFixed(2) }} °C</p>
          <p class="weather-data">Weather: {{ weather.weather[0].description }}</p>
          <p class="weather-data">Humidity: {{ weather.main.humidity }}%</p>
          <p class="weather-data">Wind Speed: {{ weather.wind.speed }} m/s</p>
        </div>
      </div>
    </q-page-container>
  </q-layout>
</template>

<script>
import axios from 'axios';

export default {
  data() {
    return {
      city: '',
      weather: null,
      loading: false,
      error: null,
      apiKey: '2e654253096d24edabbd2498fdf6489d', // Replace with your OpenWeatherMap API key
      taskLinks: [
        { name: 'Task 1', link: 'https://sindi1.netlify.app/' },
        { name: 'Task 2', link: 'https://sindimaharani-223510651-t2-pbk.netlify.app/' },
        { name: 'Task 3', link: 'https://sindimaharani-223510651-pbk-t3.netlify.app/' },
        { name: 'Task 4', link: 'https://sindimaharani223510651tugas4prakpbk.netlify.app/' },
        { name: 'Task 5', link: 'https://tugas5sindycantik.netlify.app//' },
        { name: 'Task 6', link: 'https://tugas6sinduy.netlify.app/' },
        { name: 'Task 7', link: 'https://sindimaharanitugas7prakpbk4g.netlify.app/' },
      ],
    };
  },
  methods: {
    async fetchWeather() {
      if (this.city) {
        this.loading = true;
        this.error = null;
        this.weather = null;
        try {
          const response = await axios.get(
            `https://api.openweathermap.org/data/2.5/weather?q=${this.city}&appid=${this.apiKey}`
          );
          this.weather = response.data;
        } catch (error) {
          if (error.response && error.response.status === 404) {
            this.error = "City not found. Please try again.";
          } else {
            this.error = "An error occurred while fetching the weather data.";
          }
        } finally {
          this.loading = false;
        }
      }
    },
    goToLink(link) {
      window.open(link, '_blank');
    },
  },
};
</script>

<style scoped>
.weather {
  width: 100vw;
  height: 100vh;
  display: flex;
  justify-content: center;
  align-items: center;
  flex-direction: column;
  background-image: url('../assets/bg.jpg') !important; /* Replace with path to your image */
  background-size: cover;
  background-position: center;
  background-repeat: no-repeat;
  padding: 20px;
  box-sizing: border-box;
  font-family: Calibri, 'Segoe UI', Arial, sans-serif;
}

.weather-title {
  text-transform: uppercase;
  font-size: 2.5rem;
  margin-bottom: 20px;
  color: white;
}

form {
  margin-bottom: 20px;
  text-align: center;
}

input {
  padding: 6px;
  font-size: 1.5rem;
  margin-right: 10px;
  border: 1px solid rgb(243, 234, 234);
  border-radius: 4px;
}

button {
  padding: 6px;
  font-size: 1.5rem;
  background-color: #a21b7e8c;
  border: none;
  color: white;
  border-radius: 4px;
  cursor: pointer;
}

button:hover {
  background-color: #fa84e8;
}

.weather div {
  margin-top: 20px;
}

.weather-box {
  background-color: rgba(99, 5, 99, 0.3);
  padding: 20px;
  border-radius: 8px;
  text-align: center;
}

.weather-data {
  color: #f2ecec;
  font-family: Calibri, 'Segoe UI', Arial, sans-serif;
  font-size: 1.8rem;
  margin: 5px 0;
}

.error {
  color: rgb(99, 4, 90);
}

.text-center {
  text-align: center;
}
</style>
