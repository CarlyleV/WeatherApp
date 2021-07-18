<template>
    <div id="App" class="full">
      <main>
        <div class="search-box">
          <input type="text" 
                 class="search-bar" 
                 placeholder="Search... Press Enter" 
                 ref="query"
                 @keypress.enter="fetchWeather"
                 /><br>
                 <!-- {{weather}}<br> -->
        </div>

        <div v-if="this.weather.message == 'city not found'">
          <p>City not found.<br>Please try again.</p>
        </div>

        <div class="weather-wrap" v-if="typeof weather.main != 'undefined'">
            <watch id="locationWatch" :dataTo="weather" />
          <div class="location-box">{{ weather.name }}, {{ weather.sys.country}}</div>
           <div class="weather-box">
              <img v-bind:src = "icon" />
            
               <div class="temp">{{Math.round(weather.main.temp)}}°C</div>
              <div class="weather">{{ weather.weather[0].description}}</div>
          </div>
        </div>
      


       
      </main>
    </div>
</template>

<script>
    import watch from '@/components/watch'
    export default {
      components: {
        watch
      },
      name: 'App',
      data(){
        return{
          api_key:'6378c63e6bcc3b3c68237a6154d68191',
          url_base:'https://api.openweathermap.org/data/2.5/',
          weather:{},

        }
      },
      computed:{
        icon(){
          return `http://openweathermap.org/img/wn/${this.weather.weather[0].icon}@4x.png`
        },

      },
      methods: {
        fetchWeather(){
          fetch(`${this.url_base}weather?q=${this.$refs.query.value}&units=metric&APPID=${this.api_key}`)
          .then(res => {return res.json();})
            .then(this.setResults)
        },
        setResults(results){
          this.weather = results;
        }
      }
      
    }
</script>


<style>
#app{
  position: relative;
  height: 100%;
}

main{
  overflow: hidden;
  position: absolute;
  top:50%;
  left:50%;
  transform: translate(-50%,10%);
  text-align: center;
  box-sizing: border-box;
  border-radius: 15px;
  min-height: 700px;
  min-width: 400px;
  box-shadow: 0px 20px 30px -10px rgb(117, 117, 117);

}

.search-box{
  padding-top: 20px;
  padding-bottom: 20px;
  background-color:grey;

}

#locationWatch{
  padding-top: 20px;
}

.location-box{
  padding-top: 20px;
}
</style>
