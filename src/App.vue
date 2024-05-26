<template>
  <div class="container">
    <div class="sidebar">
      <button class="btn-city" v-for="(city,index) in cities" :key="index" @click="selectCity(city)">{{ city }}</button>
    </div>
    <div class="main-content">
      <img class="spain-flag" alt="Spain flag" src="./assets/spain.png">
      <HomePage msg="Hola a todos"/>
      <p>{{ time }}</p>
      <p>{{ current_city }}</p>
    </div>
  </div>
</template>

<script>
import HomePage from './components/Home.vue'

export default {
  name: 'App',
  components: {
    HomePage
  },
  data: function(){
    return {
      time:'',
      current_city:'madrid',
      cities:[
        'Madrid',
        'Berlin',
        'Paris',
        'rome'
      ]
    }
  },
  mounted() {
    this.updateTime();
    this.timer = setInterval(this.updateTime, 50000); 
  },
  methods:{
    updateTime() {
      fetch('http://worldtimeapi.org/api/timezone/europe/'+this.current_city)
        .then(response => {
          if (!response.ok) {
            throw new Error('Network response was not ok :' + response.statusText);
          }
          return response.json();
        })
        .then(data => {
          const datetime=data.datetime.split('T')
          const hours=datetime[1].split(':')[0]
          const minutes=datetime[1].split(':')[1]
          this.time = `${hours}:${minutes}`
        })
        .catch(error => {
          console.error('There was a problem with the fetch operation:', error);
        });
    },
    beforeDestroy() {
      clearInterval(this.timer);
    },
    selectCity(city){
      this.current_city=city
      this.updateTime();
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
.container{
  margin: 0;
  display: flex;
  height: 500px;
}
.sidebar{
  width: 15%;
  display: flex;
  flex-direction: column;
}
.main-content{
  text-align: center;
  width: 80%;
}
.btn-city{
  height: 25%;
  margin-top: 10px;
  background-color: rgb(231, 225, 225);
  border: 0px;
  border-radius: 10px;
}
.spain-flag{
  width: 250px;
}
</style>
