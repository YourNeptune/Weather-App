<template>
  <div id="app" :class="typeof weather.main != 'undefined' && weather.main.temp > 22 ? 'hot' : ''">
    
    <main>
      <div class="search-box">
        <input 
          type="text" 
          name="" 
          id="" 
          class="search-bar" 
          placeholder="Search..."
          v-model="query"
          @keyup.enter="trigger"
        >
        <div class="search"  @click="clickSearch" ref="sendReply">
          <svg class="searchSVG" height="30" width="30">       
          <image href="./assets/search.svg" height="30" width="30"/>
        </svg>
        </div>
      </div>

      <div class="homePage" v-if="typeof weather.main === 'undefined'">
        <h2>Weather App</h2>
      </div>

      <div class="search-results" v-if="typeof weather.main != 'undefined'">
        <div class="location-box">
          <div class="location">{{weather.name}}, {{weather.sys.country}}</div>
          <div class="date">{{dateBuilder()}}</div>
        </div>
        <div class="weather-box">
          <div class="temp">
            <span class="temp-value">{{Math.round(weather.main.temp)}}</span>
            <span class="temp-unit"><b>&#8451;</b></span>
          </div>
          <div class="weather">{{weather.weather[0].main}}</div>
          <p>{{weather.weather[0].description}}</p>
        </div>
      </div>
    </main>
  </div>
  
</template>

<script>
import config from './config'
export default {
  name: 'App',
  data(){
    return {
      api_key: config.api_key,
      url_base:'https://api.openweathermap.org/data/2.5/',
      query: '',
      weather: {}
      
    }
  },
  methods: {
    clickSearch(){
     fetch(`${this.url_base}weather?q=${this.query}&units=metric&appid=${this.api_key}`)
          .then(res => {
            return res.json();
          }).then(this.setResults);
    },
    trigger () {
      this.$refs.sendReply.click()
    },
    setResults(result){
      this.weather = result;
    },
    dateBuilder(){
      let d = new Date();
      let months = ['Jan','Feb','Mar','Apr','May','Jun','Jul','Aug','Sep','Oct','Nov','Dec'];
      let days = ['Sunday','Monday','Tuesday','Wednesday','Thursday','Friday','Saturday'];

      let day = days[d.getDay()];
      let month = months[d.getMonth()];
      let date = d.getDate();
      let year = d.getFullYear();

      return `${day}, ${date} ${month} ${year}`
    }
  }
  
}
</script>

<style>
@import url('https://fonts.googleapis.com/css2?family=Gothic+A1:wght@200;400;700&display=swap');
*{
  margin: 0;
  padding: 0;
  box-sizing: border-box;
}
body{
  font-family: 'Gothic A1', sans-serif;
}
#app{
  background-image: url('./assets/background2.jpg');
  background-size: cover;
  background-position: bottom;
  transition: 0.4s;
}
#app.hot{
   background-image: url('./assets/background1.jpg');
   background-size: cover;
   background-position: bottom;
  transition: 0.4s;
}
main{
  min-height: 100vh;
  padding: 50px;
  background-image: linear-gradient(to bottom, rgba(0,0,0,0), rgba(0,0,0,0.45));
}
.search-box{
  width: 100%;
  display: flex;
  align-items: center;
}
.search-box .search-bar{
  color: rgb(96, 96, 96);
  width: 100%;
  padding:13px;
  border: none;
  border-radius: 15px 0px 0px 15px;
  background-color: rgba(255,255,255,0.5);
  box-shadow: 0px 2px 6px 0px rgba(79, 79, 79, 0.76);
  font-size: 20px;
  transition: 0.4s;

}
.search{
  color: rgb(96, 96, 96);
  padding:7px;
  border: none;
  background-color: rgba(255, 255, 255, 0.15);
  box-shadow: 0px 2px 6px 0px rgba(60, 60, 60, 0.75);
  border-radius: 0px 15px 15px 0px;
  cursor: pointer;
}
.search:hover{
  background-color: rgba(120, 119, 119, 0.173);
  box-shadow: 0px 2px 6px 0px rgba(79, 79, 79, 0.76);
}
.search-bar:focus{
  box-shadow: 0px 2px 16px rgba(79, 79, 79, 0.86);
  background-color: rgba(255,255,255,0.7);
}

.search-results, .homePage {
    text-align: center;
    color: aliceblue;
}
.homePage{
  font-size: 40px;
  padding-top: 170px;
  text-shadow: 1px 2px 10px rgba(0,0,0,0.25);
}
.location{
  font-size: 30px;
  font-weight: 700;
  padding-top: 20px;
  text-shadow: 1px 2px 10px rgba(0,0,0,0.25);
}
.date{
  font-size: 17px;
  font-weight: 200;
  font-style: italic;
}
.temp{
  display:inline-block;
  padding:20px 50px;
  background-color: rgba(255,255,255,0.25);
  border-radius: 16px;
  margin: 30px 0px;
  box-shadow:  4px 5px 10px rgba(79, 79, 79, 0.26);
  text-shadow: 1px 2px 10px rgba(0,0,0,0.25);
}
.temp .temp-value{
  font-size: 100px;
  font-weight: 700;
}
.temp .temp-unit{
  font-size: 50px;
}
.weather{
  font-size: 50px;
  font-weight: 700;
  text-shadow: 1px 2px 10px rgba(0,0,0,0.25);
}
</style>
