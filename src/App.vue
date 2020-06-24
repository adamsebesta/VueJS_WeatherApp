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
        <button class="button" @click="showModal = true">
          Show Modal
        </button>
        <transition name="fade" appear>
          <div class="modal-overlay" v-if="showModal" @click="showModal = false"></div>
        </transition>
        <transition name="slide" appear>
          <div class="modal" v-if="showModal">
            <h1>Hi There </h1>
            <p>sunt in culpa qui officia deserunt mollit anim id est laborum.</p>
            <button class="button black" @click="showModal = false" type="button" name="button">
              Close Modal
            </button>
          </div>
        </transition>
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
      showModal: false
    }
  },
  methods: {
    async fetchWeather(e) {
      if (e.key == "Enter") {
        let res = await fetch(`${this.url_base}weather?q=${this.query}&units=metric&appid=${this.api_key}`)
        let data = await res.json()
        return this.setResults(data);
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

  .button {
    appearance:none;
    outline: none;
    border: none;
    background: none;
    cursor: pointer;
    display: inline-block;
    margin: 0 auto;
    color: #FFF;
    margin-top: 2rem;
    padding: 15px 25px;
    border-radius: 5px;
    background-color: rgba(255, 255, 255, 0.25);
    font-size: 18px;
    font-weight: 700;
    box-shadow:  2px 2px rgba(0, 0, 0, 0.4);
    transition: 0.4s ease-out;
    display: flex
  }
  .button:hover {
      box-shadow: 4px 4px rgba(0, 0, 0, 0.6);
  }

  .modal-overlay {
    position: absolute;
    top:0;
    left:0;
    right: 0;
    bottom: 0;
    z-index: 98;
    background-color: rgba(0, 0, 0, 0.3);
  }

  .fade-enter-active,
  .fade-leave-active {
    transition: opacity 0.5s;
  }

  .fade-enter,
  .fade-leave-to {
    opacity: 0;
  }



  .modal {
    position: fixed;
    top: 50%;
    left: 50%;
    transform: translate(-50%, -50%);
    z-index: 99;

    width: 100%;
    max-width: 400px;
    background-color: #FFF;
    border-radius: 16px;

    padding: 25px;
  }
  .modal h1 {
    color: #222;
    font-size: 32px;
    font-weight: 900;
    margin-bottom: 15px;
   }

  .modal p {
    color: #666;
    font-size: 18px;
    font-weight: 400;
    margin-bottom: 15px;
   }

   .black {
     color: black;
   }



</style>
