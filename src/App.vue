<template>
  <div id="app" class="default"
    :class="(typeof weather.main != 'undefined' && weather.main.temp < 30 && weather.main.temp > 2)
    ? (weather.weather[0].main == 'Clouds'
    ? 'clouds'
      : (weather.weather[0].main == 'Clear')
        ? 'clear'
          :(weather.weather[0].main == 'Foggy')
            ? 'foggy'
              : (weather.weather[0].main == 'Rain')
                ? 'rain'
                  : ('default'))  
                    : (typeof weather.main != 'undefined' && weather.main.temp > 29)
                      ? 'warm'
                        :(typeof weather.main != 'undefined' && weather.main.temp <= 2)
                          ? 'snow'
                            : ''">
    <main class="container">
      <div class="search-box">
        <input 
          type="text" 
          class="search-bar" 
          placeholder="Search..."
          v-model="query"
          @keypress="fetchWeather"
        />
      </div>

      <div class="weather-wrap" v-if="typeof weather.main != 'undefined'">
        <div class="location-box">
          <div class="location">{{ weather.name }}, {{ weather.sys.country }}</div>
          <div class="date">{{ dateBuilder() }}</div>
        </div>

        <div class="weather-box">
          <div class="temp">{{ Math.round(weather.main.temp) }}°c</div>
          <div class="weather">{{ weather.weather[0].main }}</div>
        </div>
      </div>
      <div v-if="typeof weather.main == 'undefined'" class="weather-app">
        <h1 class="weather-title"> Weather App</h1>
        <div class="time-container">
          <div class="time" id="time">{{ clock() }}</div>
          <div class="date" id="date">{{ dateBuilder() }}</div>
        </div>
        <p class="weather-p">Programmed by: <a class="weather-a" href="https://github.com/allencarlosdev" target="_blank" rel="noopener noreferrer">Carlos Allen 😎👍</a></p>
      </div>
    </main>
  </div>
</template>

<script>
export default {
  name: 'app',
  data () {
    return {
      api_key: 'd318c1568848540b4f30e37598085539',
      url_base: 'https://api.openweathermap.org/data/2.5/',
      query: '',
      weather: {},
    }
  },
  methods: {
    fetchWeather (e) {
      if (e.key == "Enter") {
        fetch(`${this.url_base}weather?q=${this.query}&units=metric&APPID=${this.api_key}`)
          .then(res => {
            return res.json();
          }).then(this.setResults);
      }
    },
    setResults (results) {
      this.weather = results;
    },
    dateBuilder () {
      const d = new Date();
      let months = ["January", "February", "March", "April", "May", "June", "July", "August", "September", "October", "November", "December"];
      let days = ["Sunday", "Monday", "Tuesday", "Wednesday", "Thursday", "Friday", "Saturday"];
      let day = days[d.getDay()];
      let date = d.getDate();
      let month = months[d.getMonth()];
      let year = d.getFullYear();
      return `${day} ${date} ${month} ${year}`;
    },
    clock(){
      setInterval(() => {
        const local = new Date();
            time.innerHTML = local.toLocaleTimeString();
      }, 1000);
    }
  }
}
</script>>

<style>
* {
  margin: 0;
  padding: 0;
  box-sizing: border-box;
}
  body{
    font-family: 'montserrat', sans-serif;
  }

  .container {
    display: flex;
    flex-direction: column;
    align-items: center;
  }

  #app.default {
    background: #4CB8C4;
    background: -webkit-linear-gradient(to right, #3CD3AD, #4CB8C4);
    background: linear-gradient(to right, #3CD3AD, #4CB8C4);
    background-position: bottom;
    transition: 0.4s;
  }

  #app.warm {
    background-image: url('@/assets/warm.jpg');
  }

  #app.rain {
    background-image: url('@/assets/rain.jpg');
  }

  #app.clouds {
    background-image: url('@/assets/clouds.jpg');
  }

  #app.clear {
    background-image: url('@/assets/clear.jpg');
  }

  #app.snow {
    background-image: url('@/assets/snow.jpg');
  }

  #app.foggy {
    background-image: url('@/assets/foggy.jpg');
  }
  .weather-app{
    display: flex;
    flex-direction: column;
    justify-content: space-around;
    width: 40vw;
    height: 85vh;
    align-items: center;
    background: rgba( 255, 255, 255, 0.25 );
    box-shadow: 0rem 0rem 1rem rgba(0, 0, 0, 0.25);
    backdrop-filter: blur( 0.25rem );
    -webkit-backdrop-filter: blur( 0.25rem);
    border-radius: 2rem;
    border: 0.063rem solid rgba( 255, 255, 255, 0.18 );
  }
  .weather-title{
    color: #fff;
    font-weight: 600;
    font-size: 5rem;
    text-shadow: 0.063rem 0.188rem rgba(0, 0, 0, 0.25);
  }
  .weather-p{
    color: #fff;
    text-shadow: 0.063rem 0.188rem rgba(0, 0, 0, 0.25);
  }

  .weather-a{
    text-decoration: none;
    color: #fff;
  }

  main {
    min-height: 100vh;
    padding: 1.563rem;
    background-image: linear-gradient(to bottom,rgba(0, 0, 0, 0.25), rgba(0, 0, 0, 0.3));
  }

  .search-box {
    width: 100%;
    margin-bottom: 1.875rem;
  }

  .search-box .search-bar{
    display: block;
    width: 100%;
    padding: 0.938rem;
    color: #313131;
    font-size: 1.25rem;
    appearance: none;
    border: none;
    outline: none;
    background: none;
    background-color: rgba(255, 255, 255, 0.5);
    border-radius: 1rem;
    transition: 0.4s;
    box-shadow: 0rem 0rem 1rem rgba(0, 0, 0, 0.25);
  }

  .search-box .search-bar:focus {
    box-shadow: 0rem 0rem 1.5rem rgba(0, 0, 0, 0.25);
    background-color: rgba(255, 255, 255, 0.75);
  }

  .location-box .location {
    color: #fff;
    font-size: 2rem;
    font-weight: 500;
    text-align: center;
    text-shadow: 0.063rem 0.188rem rgba(0, 0, 0, 0.25);
  }

  .location-box .date{
    color: #fff;
    font-size: 1rem;
    font-weight: 300;
    font-style: italic;
    text-align: center;
  }

  .weather-wrap{
    display: flex;
    flex-direction: column;
    justify-content: center;
    padding: 2rem;
    border-radius: 1rem;
    background-color: rgba(255, 255, 255, 0.15);
    box-shadow: 0rem 0rem 1rem rgb(0 0 0 / 25%);
    width: 50vw;
    height: 80vh;
  }

  .weather-box {
    display: flex;
    flex-direction: column;
    text-align: center;
  }

  .weather-box .temp {
    display: inline-block;
    color: #fff;
    padding: 1rem;
    font-size: 6rem;
    font-weight: 900;
    text-shadow: 0.188rem 0.375rem rgba(0, 0, 0, 0.25);
    background-color: rgba(255, 255, 255, 0.25);
    border-radius: 1rem;
    margin: 5rem 10rem;
    box-shadow: 0.188rem 0.375rem rgba(0, 0, 0, 0.25);

  }

  .weather-box .weather {
    color: #fff;
    font-size: 3rem;
    font-weight: 700;
    font-style: italic;
    text-shadow: 0.188rem 0.375rem rgba(0, 0, 0, 0.25);
  }

  .time-container{
    display: flex;
    flex-direction: column;
    align-items: center;
    width: 80%;
    border-radius: 1rem;
    background-color: rgba(255, 255, 255, 0.25);
    padding: 0.5rem;
    box-shadow: 0.188rem 0.375rem rgba(0, 0, 0, 0.25);
  }

  .time{
    font-size: 6rem;
    font-weight: 800;
    color: #fff;
  }

  .date{
    color: #fff;
  }
  @media screen and (max-width: 1250px) {
  
    .weather-app{
      width: 85vw;
      height: 75vh;
    }

    .weather-title{
      font-size: 2.5rem;
    }

    .weather-wrap{
      width: 85vw;
      height: 80vh;
    }

  .weather-box .temp {
    margin: 5rem 0rem;
  }

  .time{
    font-size: 5rem;
  }
}

  @media screen and (max-width:350px){
    .weather-title{
      font-size: 2rem;
    }

    .weather-p{
      font-size: 0.7rem;
    }

    .weather-box .temp {
      font-size: 4.5rem;
    }

    .time{
    font-size: 2em;
  }
  }

  
</style>
