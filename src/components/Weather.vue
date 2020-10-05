<template>
<div id="weather" :class="typeof weather.main !='undefined' && 
    weather.main.temp > 16 ?'warm' : ''">
  <main>
    <div class="search-box">
      <input 
      type="text" 
      class="search-bar" 
      placeholder="Search..."
      v-model="query"
      @keyup.enter="fetchWeather"
      />

    </div>

    <div class="weather-wrap" v-if="typeof weather.main != 'undefined'">
      <div class="location-box">
        <div class="location">{{ weather.name }}, {{ weather.sys.country }}</div>
        <div class="date">{{ dateBuilder() }}</div>
      </div>

      <div class="weather-box">
        <div class="temp">{{ Math.round(weather.main.temp) }}°</div>
        <div class="feels">Feels like {{ Math.round(weather.main.feels_like) }}°</div>
        <div class="weather">{{ weather.weather[0].main }}</div>
        <div class="wind">{{ weather.wind.speed }} m/s</div>
      </div>
    </div>
  </main>
</div>
</template>

<script>
export default {
  name: 'Weather',
  data() {
    return{
      api_key: '694461054818770ae68d135e374eed9d',
      url_base: 'https://api.openweathermap.org/data/2.5/',
      query: '',
      weather: {}
    }
  },
  methods: {
    fetchWeather() {
      fetch(`${this.url_base}weather?q=${this.query}&units=metric&appid=${this.api_key}`)
        .then(res => {
          return res.json();
        }).then(this.setResults);
    },
    setResults (results) {
      this.weather = results;
    },
    dateBuilder () {
      let d = new Date();
      let months = [
        "January", "February", "March", "April", "May", "June",
        "July", "August", "September", "October", "November", "December"
      ];
      let days = [
        "Sunday", "Monday", "Tuesday", "Wednesday", "Thursday",
        "Friday", "Saturday"
      ];
      let day = days[d.getDay()];
      let date = d.getDate();
      let month = months[d.getMonth()];
      let year = d.getFullYear();

      return `${day} ${date} ${month} ${year}`;
    }
  }
}
</script>

<style>
#weather{
  background-color: #49CECF;
  background-image: url('../assets/cold.png');
  background-size: cover;
  background-position: bottom;
  transition: .4s;
}
#weather.warm{
  background-image: url('../assets/warm.png');
}
main{
  min-height: 100vh;
  padding: 25px;
  background-image: linear-gradient(to bottom, rgba(0,0,0,0.25), rgba(0,0,0,0.75) );
}
.search-box{
  width: 100%;
  margin-bottom: 30px;
}
.search-box .search-bar{
  display: block;
  width: 100%;
  padding: 15px;
  color: #313131;
  font-size: 20px;
  appearance: none;
  border: none;
  outline: none;
  background: none;
  background-color: rgba(255,255,255,0.5);
  border-radius: 16px;
  transition: .4s;
}
.search-box .search-bar:focus{
  background-color: rgba(255,255,255,0.75);
}

.location-box .location{
  color: #fff;
  font-size: 32px;
  font-weight: 500px;
  text-align: center;
  text-shadow: 1px 3px blur rgba(0,0,0,0.25);
}

.location-box .date{
  color: #fff;
  font-size: 20px;
  font-weight: 300px;
  text-align: center;
}

.weather-box{
  text-align: center;
}

.weather-box .temp{
  display: inline-block;
  color: #fff;
  font-size: 102px;
  font-weight: 900;
}
.weather-box .weather{
  color: #fff;
  font-size: 30px;
  font-weight: 700;
}
.weather-box .feels{
  color: #fff;
  font-size: 30px;
  font-weight: 700;
}
.weather-box .wind{
  color: #fff;
  font-size: 30px;
  font-weight: 700;
}
</style>
