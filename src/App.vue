<template>
  <div id="app" :class="typeof weather.main != 'undefined' && weather.main.temp - 273.15 > 16 ? 'warm' : 'cold'">
    <main>
      <div class="search-box">
        <input
            type="text"
            class="search-bar"
            placeholder="Search..."
            v-model="query"
            @keypress.enter="getWeather"
        />
      </div>
      <div class="weather-wrap" v-if="typeof  weather.main != 'undefined'">
        <div class="location-box">
          <div class="location"> {{ weather.sys.country }}, {{ weather.name }}</div>
          <div class="data">{{ dateBuilder() }}</div>
        </div>
        <div class="weather-box">
          <div class="temp">{{ Math.round(weather.main.temp - 273.15) }}&deg;C</div>
          <div class="weather">{{ weather.weather[0].main }}</div>
        </div>
      </div>
    </main>
  </div>
</template>

<script>
export default {
  data() {
    return {
      api_key: "ef22e02d9f55276ab6a271eed2d15713",
      url_base: "https://api.openweathermap.org/data/2.5/",
      query: 'tashkent',
      weather: {}
    }
  },
  methods: {
    async getWeather(event) {
      await fetch(`${this.url_base}weather?q=${this.query}&appid=${this.api_key}`)
          .then(res => {
            return res.json()
          }).then(this.setResult)
      this.query = ""
    },
    setResult(result) {
      this.weather = result
    },
    dateBuilder() {
      let dt = new Date()
      let months = ["Yanvar", "February", "March", "April", "May", "June", "July", "August", "September", "November", "December"]
      let days = ["Sunday", "Monday", "Tuesday", "Wednesday", "Thursday", "Friday", "Saturday"]
      let day = days[dt.getDay()];
      let date = dt.getDate()
      let month = months[dt.getMonth()];
      let year = dt.getFullYear()
      return `${month} ${date} ${year}. ${day}`

    }
  },
  created() {
    this.getWeather()
  }
}
</script>

<style>
* {
  margin: 0;
  padding: 0;
  box-sizing: border-box;
}

body {
  font-family: 'Montserrat', sans-serif;
}

#app {
  background-size: cover;
  transition: .5s;
}

#app.warm {
  background: url("assets/sunny.jpg") center;
}

#app.cold {
  background: url("assets/cloudy.jpg") center;
}

main {
  min-height: 100vh;
  padding: 25px;
  background-image: linear-gradient(to bottom, rgba(0, 0, 0, 0.25), rgba(0, 0, 0, 6));
  display: grid;
  grid-template-columns: 50% 50%;
}

.search-box {
  width: 90%;
}

.search-box .search-bar {
  margin-top: 30vh;
  display: block;
  width: 100%;
  padding: 12px;
  color: #353535;
  font-size: 15px;
  border: none;
  box-shadow: 0 0 8px rgba(0, 0, 0, .25);
  background: rgba(255, 255, 255, 0.6);
  outline: none;
  appearance: none;
  border-radius: 0 16px 0 16px;
  transition: .4s;
}

.search-box .search-bar:focus {
  box-shadow: 0 0 16px rgba(0, 0, 0, .25);
  background-color: rgba(255, 255, 255, .8);
  border-radius: 16px 0 16px 0;
}

.location-box .location {
  margin-top: 10vh;
  color: white;
  font-size: 32px;
  font-weight: 500;
  text-align: center;
  text-shadow: 1px 3px rgba(0, 0, 0, .25);
}

.location-box .data {
  color: white;
  font-size: 20px;
  font-weight: 300;
  font-style: italic;
  text-align: center;
}

.weather-wrap {
  transition: .5s;
}

.weather-box {
  text-align: center;
}

.weather-box .temp {
  display: inline-block;
  padding: 10px 25px;
  color: white;
  font-size: 100px;
  font-weight: 900;
  text-shadow: 3px 6px rgba(0, 0, 0, .25);
  background-color: rgba(255, 255, 255, .25);
  border-radius: 15px;
  margin: 30px 0;
  box-shadow: 3px 6px rgba(0, 0, 0, .25);
}

.weather-box .weather {
  color: white;
  font-size: 28px;
  font-weight: 700;
  font-style: italic;
  text-shadow: 3px 6px rgba(0, 0, 0, .25);
}
</style>
