<template>
  <div id="app" :class="typeof weather.main != 'undefined' && weather.main.temp > 15 ? 'warm' : ''">
    <main>
      <div class="search-box">
          <input name="" 
          type="text" 
          id="" 
          class="search-bar"
          placeholder="Zadaj mesto"
          v-model= "query"
          @keypress="fetch_weather"
          />

      </div>
      <div class="weather-wrap" v-if="typeof weather.main != 'undefined' ">
        <div class="location-box">
          <div class="location">{{ weather.name }}, {{ weather.sys.country }}</div>
          <div class="date">{{ dateBuilder() }}</div>
          </div>
        <div class="weather-box">
          <div class="temp">{{ Math.round(weather.main.temp) }}°C</div>
          <div class="weather-image"><img id="wicon" alt="Weather icon" v-bind:src="`https://openweathermap.org/img/w/${weather.weather[0].icon}.png`"> </div>
          <div class="weather">{{ weather.weather[0].main }}</div>
          <div class="weather-description">{{ weather.weather[0].description }}</div>
        </div>
      </div>
    </main>
  </div>
</template>

<script>

export default {
  name: 'app',
  data(){
    return{
      api_key: '0bf3d33de721f500baa2f7ff4135979a',
      url_base: 'https://api.openweathermap.org/data/2.5/',
      query: '',
      weather: {}
    }
  },
  methods: { 

    fetch_weather(e) {
      if (e.key == "Enter") {
        fetch( `${this.url_base}weather?q=${this.query}&units=metric&APPID=${this.api_key}`)
          .then(res => {
            return res.json();
          }).then(this.setResults);
      }
    },
    setResults(results) {
      this.weather = results;
    },
    dateBuilder(){
      let d = new Date();
      let months = ["January", "February", "March", "April", "May", "June", "July", "August", "September", "October", "November", "December"];
      let days = ["Sunday", "Monday", "Tuesday", "Wednesday", "Thursday", "Friday", "Saturday"];
      let day = days[d.getDay()];
      let date = d.getDate();
      let month = months[d.getMonth()];
      let year = d.getFullYear();
      return `${day} ${date} ${month} ${year}`;

    },
  }

}


</script>

<style scoped>
*{
  margin: 0;
  padding: 0;
  font-family: 'montserrat', sans-serif;

}
body {
  display: flex;
  justify-content: center;
  align-items: center;
  overflow: hidden;
}

#app{
  background-image: url('./assets/cold.jpg');
  border-radius: 30px;
  border: 10px solid black;
  background-size: cover;
  background-position: bottom;
  
  max-width: 60vh;
  max-height: 95vh;
  vertical-align: middle;
  scroll-behavior: auto;
  position: absolute;

  
  margin: 0 auto;
  left: 0;
  right: 0;
  
}
#app.warm {
  background-image: url('./assets/warm.jpg');
}
#app.freze {
  background-image: url('./assets/freze.png');
}

main{
  min-height: 95vh;
  background-image: linear-gradient(to bottom, rgba(0, 0, 0, 0.25), rgba(0, 0, 0, 0.25));
}

.search-box{
  width: 100%;
  margin-bottom: 30px;
  display: grid;

}
.search-box .search-bar{
  display: block;
  margin: 0 auto;
  margin-top: 5vh;
  width: 80%;
  padding: 15px;
  color: #313131;
  font-size: 20px;
  box-shadow: 0px 0px 16px rgba(0, 0, 0, 0.25);

  appearance: none;
  border: none;
  outline: none;

  background-color: rgba(255, 255, 255, 0.25);
  border-radius: 10px;
  transition: 0.4s;
}
.search-box .search-bar:focus{
  background-color: rgba(255, 255, 255, 0.75);
  box-shadow: 0px 0px 16px rgba(0, 0, 0, 0.25)
}

.location-box .location{
  color: #ffffff;
  font-size: 32px;
  font-weight: 500;
  text-align: center;
  text-shadow: 1px 3px rgba(0, 0, 0, 0.25)
}
.location-box .date{
  color: #ffffff;
  font-size: 20px;
  font-weight: 300;
  font-style: italic;
  text-align: center;
}
.weather-box{
  text-align: center;
}

.weather-box .temp{
  display: inline-block;
  padding: 10px 25px 0px 25px;
  color: #ffffff;
  font-size: 102px;
  font-weight: 900;

  text-shadow: 3px 6px rgba(0, 0, 0, 0.25);
  background-color: rgba(255, 255, 255, 0.25);
  border-radius: 16px;
  margin: 30px 0px;

  box-shadow: 3px 6px rgba(0, 0, 0, 0.25)
}
.weather-box .weather{
  color:#ffffff;
  font-size: 35px;
  font-weight: 300;
  font-style: italic;
  text-shadow: 3px 6px rgba(0, 0, 0, 0.25);
}
.weather-box .weather-description{
  color: #ffffff;
  font-size: 15px;
  font-weight: 300;
  font-style: italic;
}
.weather-box .weather-image{
  position: center;
}

@media screen and (max-width: 768px){
  #app{
    border-radius: 0px;
    border: none;
    max-width: 100vw;
    max-height: 100vh;
    
  }
  main{
    min-height: 100vh;
  }

}


</style>
