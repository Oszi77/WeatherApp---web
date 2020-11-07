<template>

   <div id="app">
     <header>
        <img class="logo" src="./assets/Logó.svg" alt="">
      </header>

    <main>
      <p>{{req}}</p>

      <div class="search-box">
        <input 
          type="text" 
          class="search-bar" 
          placeholder="Írj be egy várost ..."
          v-model="query" 
          @keypress="fetchWeather"
          />
      </div>

      <div class="forcast-box" v-if="typeof req.main != 'undefined'">
        <div class="row">
          <div class="loc-time">
            <div class="location">{{req.name}}</div>
            <div class="time">{{time}}</div>
          </div>
          <div class="Date">
            <div class="day">{{dating.day}}</div>
            <div class="mounth">{{dating.mounth}}. {{dating.date}}</div>
          </div>
        </div>

        <div class="row">
          <div class="temperature">
            <span id="ig">
              <img src="./assets/Eső.png" alt="">
            </span>
            <span class="degree">{{Math.round(req.main.temp)}}°C</span>
          </div>

          <div class="weather">
            <div class="state">{{req.weather[0].main}}</div>
            <span class="humidity-img">
              <img src="./assets/esernyő.png" alt="">
            </span>
            <span class="humidity">{{req.main.humidity}}%</span>
            <span class="min">{{Math.round(req.main.temp_min)}}</span><span><img class="aditional-degree" src="./assets/arrow-down.svg" alt=""></span>
            <span class="max">{{Math.round(req.main.temp_max)}}</span><span><img class="aditional-degree" src="./assets/arrow-up.svg" alt=""></span>
          </div> 
        </div>

      </div>
      <!-- <div class="fiveday-forcast-box">
        <div class="loc-time">
          <div class="location">{{req.name}}</div>
          <div class="time">12:28<span>PM</span></div>
        </div>
      </div> -->

     
    </main>

    <footer>
      <div>Created by Oszkár Design</div>
    </footer>


   </div>
</template>

<script>
  import { gsap } from "gsap/dist/gsap";
  import axios from "axios";

  export default {
    name: 'app',
    data(){
      return{
        title: "Időjárás előrejelzések.",
        query: '',
        req: {},
        dating: this.dateBuilder(),
        time: null
      };
    },
    methods:{
      showTime(){
        //Pontos idő
        let d = new Date();
        let h = d.getHours();//0 - 23
        let m = d.getMinutes();//0 - 59
        let s = d.getSeconds();//0 - 59

        let time = h +":"+ m +":"+s;

        this.time = time; 
    },
        fetchWeather(e){
          const options = {
          method: 'GET',
          url: 'https://api.openweathermap.org/data/2.5/weather',
          params: {
            q: this.query,
            lat: '0',
            lon: '0',
            id: '2172797',
            appid: '40e4a5b88592463691a3a77dcd9acfa1',
            lang: 'null',
            units: 'metric',
            mode: null
          },
        };
        if (e.key == 'Enter') {
           axios.request(options)
           .then(response => (this.req = response.data))
           //.then(response => (this.apiData = response.data))
           .catch(error => (console.error(error)));
           setInterval(this.showTime, 100);
           }
        },
        dateBuilder(){
          let d = new Date();
          let mounths = ["Január", "Február", "Március", "Április", "Május", "Junius",
                        "Júlis", "Augustus", "Szeptember", "Október", "November", "December"];
          let days = ["Vasárnap", "Hétfő", "Kedd", "Szerda", 
                      "Csütörtök", "Péntek", "Szombat"];
          let day = days[d.getDay()];
          let date = d.getDate();
          let mounth = mounths[d.getMonth()];

          return {
            day: day,
            date: date,
            mounth: mounth 
            };
        }
    },
   mounted(){
     gsap.from('.logo', {opacity: 0, duration: 1, y: -50, ease: 'elastic(1, 0.3)'});
   },
}

</script>

<style>
@import url('https://fonts.googleapis.com/css2?family=Open+Sans&display=swap');
  *{
    padding: 0;
    margin: 0;
    border: 0;
    outline: none;
    font-size: 14px;
}
body{
  font-family: 'Open Sans', sans-serif;
  font-weight: 400;
  color: #fff;
}

#app{
  background-image: url('./assets/szarva-nap.jpg');
  background-size: cover;
  background-position: bottom;
  transition: 0.4s;
}

main{
  min-height: 1080px;
  padding: 25px;
}
header{
  widows: 100%;
  background-color: rgba(255, 255, 255, 0.5);
  padding: 10px;

}
.search-box{
  width: 100%;
  
}
.search-box .search-bar{
  display: block;
  max-width: 800px;
  margin: 30px auto;
  padding: 15px;
  font-size: 20px;
  font-style: italic;
  box-shadow: 3px 3px 6px rgba(0, 0, 0, 0.25);
  background-color: rgba(255, 255, 255, 0.5);
  border-radius: 16px 0px 16px 0px;
}
.search-box .search-bar:focus{
  box-shadow: 3px 3px 12px rgba(0, 0, 0, 0.25);
  background-color: rgba(255, 255, 255, 0.75);
}
.forcast-box{
  margin-left: 200px;
  max-width: 520px;
  max-height: 290px;
  padding: 30px;
  border-radius: 10px;
  background-color: rgba(255, 255, 255, 0.5);
  box-shadow: 3px 3px 6px rgba(0, 0, 0, 0.25);
  font-style: italic;
  font-weight: 400;
}
.row{
  display: flex;
}
.loc-time .location{
  text-shadow: 3px 3px 6px rgba(0, 0, 0, 1);
  font-size: 30px;
  font-weight: 600;
}
.loc-time .time{
  text-shadow: 3px 3px 6px rgba(0, 0, 0, 1);
  font-size: 30px;
}
.Date{
  margin-left: 330px;
}
.Date .day{
  text-shadow: 3px 3px 6px rgba(0, 0, 0, 1);
  font-size: 15px;
  font-weight: 600;
}
.Date .mounth{
  color: #9E0A54;
  font-weight: 300;
  font-size: 10px;
}
.weather{
  margin-left: 30px;
  margin-top:  50px;
}
.temperature{
  margin-top: 50px;
}
.temperature .degree{
  text-shadow: 3px 3px 6px rgba(0, 0, 0, 1);
  font-size: 70px;
}
.state, .humidity, .min, .max{
  text-shadow: 3px 3px 6px rgba(0, 0, 0, 1);
  font-size: 30px;
}
.weather .min, .max{
  margin-left: 30px;
}
.aditional-degree{
  font-size: 30px;
  color: #fff;
}

.fiveday-forcast-box{
  max-width: 1123px;
  max-height: 290px;
  padding: 20px;
  margin-top: 20px;
  border-radius: 10px;
  background-color: rgba(255, 255, 255, 0.5);
  box-shadow: 3px 3px 6px rgba(0, 0, 0, 0.25);
}



footer{
  width: 100%;
  padding: 5px;
  background-color: rgba(255, 255, 255, 0.25);
  font-size: 30px;
  font-style: normal;
  font-weight: 600;
}

</style>
