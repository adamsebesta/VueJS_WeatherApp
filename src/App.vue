<template>
  <div id="app" :class="typeof weather.main != 'undefined' && weather.main.temp > 16 ?
  'warm' : ''">
    <main>
      <div class="search-box">
        <input type="text"
               class="search-bar"
               name=""
               value=""
               placeholder="Search..."
               v-model='query'
               @keypress="fetchWeather"
               />
      </div>
      <div class="weather-wrap" v-if="typeof weather.main != 'undefined'">
        <div class="location-box">
          <div class="location">{{ weather.name }}, {{weather.sys.country}}</div>
          <div class="date"> {{ dateBuilder() }} </div>
        </div>
        <div class="weather-box">
          <div class="temp">{{ Math.round(weather.main.temp) }}℃</div>
          <div class="weather">{{weather.weather[0].main}}</div>
        </div>
        <button class="button" @click="showModal = true" type="button" name="button">
          Show Modal
        </button>
        <transition name="fade" appear>
        </transition>
          <div class="model-overlay" v-if="showModal"></div>
      </div>
    </main>
  </div>
</template>

<script>


export default {
  name: 'App',
  data () {
    return {
      api_key:'fa0e55e996eeb489a1290979dd97c140',
      url_base: 'https://api.openweathermap.org/data/2.5/',
      query: '',
      weather: {},
      showModal: true
    }
  },
  methods: {
    fetchWeather (e) {
      if (e.key == "Enter") {
        fetch(`${this.url_base}weather?q=${this.query}&units=metric&appid=${this.api_key}`)
          .then(res => {
            return res.json()
          }).then(this.setResults)
      }
    },
    setResults (results) {
      this.weather = results;
    },
    dateBuilder () {
      let d = new Date();
      let months = ["January", "February", "March", "April", "May", "June", "July", "August", "September", "October", "November", "December"];
      let days = ["Sunday", "Monday", "Tuesday", "Wednesday", "Thursday", "Friday", "Saturday"];
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
 * {
   margin: 0;
   padding: 0;
   box-sizing: border-box;
 }

  body {
    font-family: 'monserrat', sans-serif;
  }

  #app {
    background-image: url(https://images.unsplash.com/photo-1536227019771-b2eac2dd6121?ixlib=rb-1.2.1&ixid=eyJhcHBfaWQiOjEyMDd9&auto=format&fit=crop&w=1291&q=80);
    background-size: cover;
    background-position: bottom;
    transition: 0.4s;
  }

  #app.warm {
    background-image: url(https://images.unsplash.com/photo-1488197047962-b48492212cda?ixlib=rb-1.2.1&ixid=eyJhcHBfaWQiOjEyMDd9&auto=format&fit=crop&w=1647&q=80)
  }

  main {
    min-height: 100vh;
    padding: 25px;
    background-image: linear-gradient(to bottom, rgba(0, 0, 0, 0.25), rgba(0, 0, 0, 0.75) );
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
      font-size: 20px;
      appearance: none;
      border: none;
      outline: none;
      background-color: rgba( 255, 255, 255, 0.5);
      box-shadow: 0px 0px 8px rgba(0, 0, 0, 0.25);
  }

  .search-box .search-bar:focus {
    background-color: rgba( 255, 255, 255, 0.75);
  }

  .location-box .location {
    color: white;
    font-size: 32px;
    font-weight: 500;
    text-align: center;
    text-shadow: 1px, 3px, rgba(0, 0, 0, 0.25);
  }

  .location-box .date {
    color: white;
    font-size: 20px;
    font-weight: 300;
    font-style: italic;
    text-align: center;
  }

  .weather-box {
    text-align: center;
  }

  .weather-box .temp {
    display: inline-block;
    padding: 10px 25px;
    color: white;
    font-size: 102px;
    font-weight: 900;
    text-shadow: 3px 6px rgba(0, 0, 0, 0.25);
    background-color: rgba(255, 255, 255, 0.25);
    border-radius: 16px;
    margin: 30px 0px;
    box-shadow: 3px 6px rgba(0, 0, 0, 0.25);
  }

  .weather-box .weather {
    color: white;
    font-size: 40px;
    font-weight: bold;
    font-style: italic;
    text-shadow: 3px 6px rgba(0, 0, 0, 0.25);
  }

  .model-overlay {

  }

  .button {
    appearance:none;
    outline: none;
    border: none;
    background: none;
    cursor: pointer;
    display: inline-block;
    margin: 0 auto;
    margin-top: 4rem;
    padding: 15px 25px;
    border-radius: 5px;
    background-color: rgba(255, 255, 255, 0.25);
    color: #FFF;
    font-size: 18px;
    font-weight: 700;
    box-shadow:  2px 2px rgba(0, 0, 0, 0.4);
    transition: 0.4s ease-out;
    display: flex
  }
  .button:hover {
      box-shadow: 4px 4px rgba(0, 0, 0, 0.6);
  }

</style>
