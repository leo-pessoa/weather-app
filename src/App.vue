<template>
  <div id="app">
    <nav>
      <router-link to="/">Home</router-link> |
      <router-link to="/about">About</router-link>
    </nav>
    <router-view/>
  </div>
</template>

<style lang="scss">
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
}

nav {
  padding: 30px;

  a {
    font-weight: bold;
    color: #2c3e50;

    &.router-link-exact-active {
      color: #42b983;
    }
  }
}
</style>
<script>
export default {
  name: 'App',
  components: {
  },
  data() {
    return {
     location: {},
      weather: [],
      displayWeather: false
    }
  },
  methods: {
    getWeather: function () {
      return this.$http.get('https://api.open-meteo.com/v1/forecast?latitude=' + this.location.latitude + '&longitude=' + this.location.latitude +'&hourly=temperature_2m').then((response) => {
        this.weather = response.body;
        console.log(this.weather)
        this.displayWeather = true
      });
    }
  },
  created() {
    if (!("geolocation" in navigator)) {
      this.errorStr = 'Geolocation is not available.';
      return;
    }

    this.gettingLocation = true;
    navigator.geolocation.getCurrentPosition(pos => {
      this.gettingLocation = false;
      this.location = pos.coords;
      console.log(this.location)
    }, err => {
      this.gettingLocation = false;
      this.errorStr = err.message;
    })
  }
}
</script>
