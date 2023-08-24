<template>
  <div class="home bg-day">
    <!-- header  -->
    <div class="header">
      <div @click="getLocation" class="location-icon">
        <i class="fa-solid fa-location-crosshairs"></i>
      </div>

      <input
        @keyup.enter="getWeatherBySearch"
        v-model="search"
        placeholder="Search"
        dark
        borderless
        type="text"
      />

      <div @click="getWeatherBySearch" class="search-icon">
        <i class="fa-solid fa-magnifying-glass"></i>
      </div>
    </div>
    <!-- body -->
    <div class="content">
      <template v-if="weatherData">
        <div>
          <div>{{ weatherData.name }}</div>
          <div>
            {{ weatherData.weather[0].main }}
          </div>
          <div>
            {{ Math.round(weatherData.main.temp / 10)
            }}<span class="text-h3 text-weight-light">&deg;C</span>
          </div>
        </div>
        <div>
          <img
            :src="`http://openweathermap.org/img/wn/${weatherData.weather[0].icon}@2x.png`"
          />
        </div>
      </template>
      <template v-else
        ><div>
          <div>Quasar<br />Weather</div>
          <button class="getLocation-btn" @click="getLocation">
            <i @click="getLocation" class="fa-solid fa-location-crosshairs"></i>
            <div>Find my location</div>
          </button>
        </div>
      </template>
    </div>

    <div class="skyline"></div>
  </div>
</template>

<script>
import axios from "axios";
export default {
  name: "HomeView",
  data() {
    return {
      search: "",
      weatherData: "",
      lat: null,
      lon: null,
      apiUrl: "http://api.openweathermap.org/geo/1.0/reverse",
      apiKey: "ff59da9d43b3ea8485eecadf9c38ff3c",
    };
  },
  methods: {
    getLocation() {
      // this.$q.loading.show();
      navigator.geolocation.getCurrentPosition((position) => {
        this.lat = position.coords.latitude;
        this.lon = position.coords.longitude;
        this.getWeatherByCoords();
      });
    },
    async getWeatherByCoords() {
      const response = await axios.get(
        `https://api.openweathermap.org/data/2.5/weather?lat=${this.lat}&lon=${this.lon}&appid=${this.apiKey}`
      );
      this.weatherData = response.data;
    },

    async getWeatherBySearch() {
      try {
        const res = await axios.get(
          `https://api.openweathermap.org/data/2.5/weather?q=${this.search}&appid=${this.apiKey}`
        );
        this.weatherData = res.data;
      } catch (error) {
        alert(error.message);
      }
    },
  },
  // computed: {
  //   bgClass() {
  //     if (this.weatherData) {
  //       if (this.weatherData.weather[0].icon.endsWith("n")) {
  //         return "bg-night";
  //       } else {
  //         return "bg-day";
  //       }
  //     }
  //   },
  // },
};
</script>
<style>
body {
  margin: 0;
}
.home {
  background: linear-gradient(to bottom, #136a8a, #267871);
  display: flex;
  flex-direction: column;
  align-items: center;
  height: 100vh;
  color: white;
}
.home.bg-night {
  background: linear-gradient(to right, #000000, #434343);
}
.home.bg-day {
  background: linear-gradient(to right, #56ccf2, #2f80ed);
}

.header {
  display: flex;
  justify-content: space-between;
  font-size: 23px;
  padding: 20px 0;
  width: 90%;
}
.header input {
  width: 80%;
  background-color: transparent;
  border: none;
}
.header input:focus {
  outline: none;
  border: none;
}

.content {
  margin: 200px auto;
  font-size: 60px;
  text-align: center;
  width: 100%;
}
svg {
  cursor: pointer;
}
.content .getLocation-btn {
  display: flex;
  justify-content: center;
  align-items: center;
  width: 100%;
  padding: 50px;
  margin-top: 50px;
  background-color: transparent;
  border: none;
  font-size: 22px;
  text-transform: uppercase;
  transition: 0.7s;
  cursor: pointer;
}
.content .getLocation-btn:hover {
  background-color: rgb(211 218 225 / 43%);
}
.content .getLocation-btn svg {
  margin-right: 15px;
  font-size: 50px;
}
.skyline {
  position: absolute;
  width: 100%;
  height: 181px;
  bottom: 0;
  background: url(C:\Users\Delta.co\Documents\quasar-project\public\icons\skyline.png);
  background-size: contain;
  background-position: center bottom;
}
</style>
