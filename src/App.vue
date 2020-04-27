<template>
  <div id="app">
    <div class="container">
      <div class="row">
        <h1>App</h1>

        <div class="col-12" v-if="weatherData != null">
          <!-- <nav> -->
            <div class="btn-toolbar">
            <div class=" btn-group d-flex justify-content-start " >
              <button v-for="(city, index) in cities" v-bind:key="index" v-on:click="changeIndex(index)" class="btn btn-info " v-bind:class="{ active: city.isActive }"> {{city.name}} </button>
            </div>
          </div>
          <!-- </nav> -->
          <table class="table text-left mt-1" v-if="!loading">
            <thead class="thead-dark">
              <tr><th colspan="2" scope="col">{{cities[tabIndex].name}} </th>
              </tr>
            </thead>
            <tbody>
              <tr>
                <th scope="col">Temperature</th>
                <td>{{weatherData.main.temp}}</td>
              </tr> 
              <tr><th scope="col">Atmospheric pressure</th> <td>{{weatherData.main.pressure}}</td></tr>
              <tr><th scope="col">Humidity</th> <td>{{weatherData.main.humidity}}</td>
              </tr>
              <tr><th scope="col">Wind speed</th> <td>{{weatherData.wind.speed}}</td>
              </tr> 
              <tr><th scope="col">Wind direction</th> <td>{{weatherData.wind.deg}}</td>
              </tr>
            </tbody>
          </table>
        </div>
      </div> 
    </div>
  </div>  
</template>

<script>

export default {
  name: 'App',
  data(){
    return {
      cities: [
      {
        "id": 703447,
        "name": "Kyiv",
        "isActive":true
      },
      {
        "id": 2643743,
        "name": "London",
        "isActive":false
      },
      {
        "id": 5128638,
        "name": "New York",
        "isActive":false
      },
      ],
      apiKey: 'c2dcf8ffb5cdc3f8977bfd2ae7ea4738',
      apiUrl: 'https://api.openweathermap.org/data/2.5/weather',
      weatherData:null,
      tabIndex: 0,
      loading: true,
    }
  },
  mounted(){
    this.getWeather();
    
  },
   methods:{
    changeIndex(index){
      this.loading=true;
      this.tabIndex=index;
      this.cities.forEach( (item)=>item.isActive=false);
      this.cities[this.tabIndex].isActive=true;
      this.getWeather()

    },
    getWeather(){
      this.$axios
      .get(this.apiUrl+'?q='+this.cities[this.tabIndex].name+'&units=metric'+'&appid='+this.apiKey)
      .then(response => (this.weatherData=response.data))
      .catch(error => console.log(error));
      this.loading=false;
    }
    
  }

}
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>
