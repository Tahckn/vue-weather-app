<template>
  <div id="app" 
  :class="
      typeof weather.main != 'undefined' && weather.main.temp < 16 ? 'snowy' : ''
    "
  >
    <main>
      <div class="search-box">
        <input
          type="text"
          class="search-bar"
          placeholder="Search..."
          v-model="query"
          @keypress="fetchData"
        />
      </div>
      <div class="weather-wrap" v-if="typeof weather.main != 'undefined'">
        <div class="location-box">
          <div class="location">
            {{ weather.name }} - {{ weather.sys.country }}
          </div>
          <div class="date">{{ dateNotifier() }}</div>
        </div>
        <div class="weather-box">
          <div class="temperature">{{ Math.round(weather.main.temp) }} °C</div>
          <div class="weather">{{ weather.weather[0].main }}</div>
          <div class="ankara">{{ weatherCondition }}</div>
        </div>
      </div>
    </main>
  </div>
</template>

<script>
export default {
  name: 'App',
  data() {
    return {
      api_key: '4b2adb6eb0c53aff786684ea0f3b033f',
      url_base: 'https://api.openweathermap.org/data/2.5/',
      query: '',
      weather: {},
      weatherCondition: '',
    };
  },
  methods: {
    fetchData(e) {
      if (e.key == 'Enter') {
        fetch(
          `${this.url_base}weather?q=${this.query}&units=metric&appid=${this.api_key}`
        )
          .then((response) => {
            return response.json();
          })
          .then(this.setResults);
      }
    },
    setResults(results) {
      this.weather = results;
    },
    dateNotifier() {
      let date = new Date();
      let day = date.getDay();
      let month = date.getMonth();
      let year = date.getFullYear();
      let dayNames = [
        'Sunday',
        'Monday',
        'Tuesday',
        'Wednesday',
        'Thursday',
        'Friday',
        'Saturday',
      ];
      let monthNames = [
        'January',
        'February',
        'March',
        'April',
        'May',
        'June',
        'July',
        'August',
        'September',
        'October',
        'November',
        'December',
      ];
      return `${dayNames[day]} ${date.getDate()} ${monthNames[month]} ${year} `;
    },
  },
};
</script>

<style>
:root {
  --text-color: #f5f5f5;
}
* {
  margin: 0;
  padding: 0;
  box-sizing: border-box;
}

body {
  font-family: 'Libre Franklin', sans-serif;
}

#app {
  background-image: url('./assets/images/day_clearsky.png');
  background-size: cover;
  background-position: bottom;
  transition: 0.4s;
}

#app.snowy {
  background-image: url('./assets/images/day_snow.png');
}


main {
  min-height: 100vh;
  padding: 25px;

  background-image: linear-gradient(
    to bottom,
    rgba(0, 0, 0, 0.11),
    rgba(0, 0, 0, 0.6)
  );
}

.search-box {
  width: 100%;
  margin-bottom: 30px;
}
.search-box .search-bar {
  display: block;
  width: 100%;
  padding: 15px;

  color: #313131;
  appearance: none;
  border: none;
  box-shadow: 0px 0px 8px rgba(0, 0, 0, 0.25);
  outline: none;
  background: none;
  background-color: rgba(255, 255, 255, 0.5);
  border-radius: 0px 16px 0px 16px;
  transition: 0.5s;
}

.search-box .search-bar:focus {
  box-shadow: 0px 0px 16px rgba(0, 0, 0, 0.25);
  background-color: rgba(255, 255, 255, 0.8);
  border-radius: 16px 0px 16px 0px;
}

.location-box .location {
  color: var(--text-color);
  font-size: 30px;
  font-weight: 500;
  text-align: center;
  text-shadow: 1px 3px rgba(0, 0, 0, 0.25);
}

.location-box .date {
  color: var(--text-color);
  font-size: 20px;
  font-weight: 100;
  font-style: italic;
  text-align: center;
}

.weather-box {
  text-align: center;
}

.weather-box .temperature {
  display: inline-block;
  padding: 10px 25px;
  color: var(--text-color);
  font-size: 60px;
  font-weight: 800;
  text-shadow: 3px 6px rgba(0, 0, 0, 0.15);
  background-color: rgba(255, 255, 255, 0.25);
  margin: 30px;
  border-radius: 16px;
  box-shadow: 3px 6px rgba(0, 0, 0, 0.25);
}

.weather-box .weather {
  color: var(--text-color);
  font-size: 48px;
  font-weight: 700;
}
</style>
