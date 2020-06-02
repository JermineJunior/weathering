<template>
<div id="app font-serif">
    <div class="flex items-center justify-center mt-10 ">
    <div class="search w-3/5 ml-20">
        <div class="relative w-full">
            <div class="absolute inset-y-0 left-0 flex items-center mb-1 ">
                <svg class="h-5 w-5" viewBox="0 0 18 18" fill="none" xmlns="http://www.w3.org/2000/svg">
                    <path fill-rule="evenodd" clip-rule="evenodd" d="M8 4C5.79086 4 4 5.79086 4 8C4 10.2091 5.79086 12 8 12C10.2091 12 12 10.2091 12 8C12 5.79086 10.2091 4 8 4ZM2 8C2 4.68629 4.68629 2 8 2C11.3137 2 14 4.68629 14 8C14 9.29583 13.5892 10.4957 12.8907 11.4765L17.7071 16.2929C18.0976 16.6834 18.0976 17.3166 17.7071 17.7071C17.3166 18.0976 16.6834 18.0976 16.2929 17.7071L11.4765 12.8907C10.4957 13.5892 9.29583 14 8 14C4.68629 14 2 11.3137 2 8Z" fill="#a0aec0"/>
                </svg>
            </div>
            <input class="search-input w-full rounded-full pl-8 -ml-2 py-2 mr-2 mr-4 focus:outline-none" 
            type="search" 
            v-model="city"
            @keypress="fetchWeather"
            placeholder="Type city name">
        </div>
      </div>
    </div><!-- search -->

    <div class="text-center mt-10 ml-20" v-if="typeof weather.main !='undefined'">
      <div class="">
        <div class="text-xl text-gray-100 font-semibold">{{weather.name}}, {{ weather.sys.country }}</div>
        <div class="leading-relaxed text-gray-100 text-lg italic">{{dateBuilder()}}</div>
      </div>
      <div class="card mt-4 px-4 py-3">
        <div class="">{{Math.round(weather.main.temp) }}°c</div>
       <!--  <div>Wind Speed {{weather.wind.speed }} KPH</div>  not important -->
        <div class="italic">{{weather.weather[0].main}}</div>
      </div>
  </div><!-- results -->

  <footer class="flex items-center justify-center mt-64 bottom-0">
    <div class="flex items-center justify-around px-6 py-4">
      <div class="text-white  mx-6">
        &copy; Baka Team 2020 , All Rights Reserved
      </div>
    </div>
  </footer>
</div>
</template>

<script>

export default {
  name: 'App',
  data(){
    return{
      api_key:'c1e9728f518bf2dd5717027100381b7a',
      url_base: "https://api.openweathermap.org/data/2.5/weather",
      city: "",
      weather: {},
    }
  },
  methods: {
     fetchWeather(e) {
      if (e.key == "Enter") {
        fetch(
          `${this.url_base}?q=${this.city}&units=metric&APPID=${this.api_key}`
        )
          .then(res => {
            return res.json();
          })
          .then(this.setResults);
      }
    },
    setResults(results) {
      this.weather = results;
    },
    dateBuilder() {
      let dateObject = new Date();
      let months = [
        "January","February","March","April","May","June","July",
        "August","September","October","November","December"
      ];
      let days = [
        "Sunday","Monday","Tuesday","Wednesday","Thursday","Friday","Saturday"
      ];

      let day = days[dateObject.getDay()];//convert day number to day name
      let date = dateObject.getDate();
      let month = months[dateObject.getMonth()];//convert month number to name
      let year = dateObject.getFullYear();
      return `${day} ${date} ${month} ${year}`;
    }
  }
};
</script>

<style >
  @import "assets/css/tailwind.css";
  .cover{
    background-image: url("./assets/cover2.jpg");
    background-size: cover;
    background-repeat: no-repeat;
    transition: 0.4s;
  }
</style>