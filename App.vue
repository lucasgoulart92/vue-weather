<template>
  <div id="app"
    :class="typeof weather.main != 'undefined'
    && weather.main.temp > 20 ? 'warm' : ''
    " >
    <main>
      
      <div class="weather-wrap" v-if="typeof weather.main != 'undefined'">
        <div class="location-box">
          <div class="location">{{ weather.name }}</div>
          <div class="date">{{ dateBuilder() }} </div>
        </div>

        <div class="weather-box">
          <div class="temp">{{ Math.round(weather.main.temp) }}°C</div>
        </div>
      </div>

      <div class="search-box">

        <input
          type="text"
          class="search-bar"
          placeholder="Pesquise aqui..."
          v-model="query"
          @keypress="fetchWeather"
        />
        <p>Digite acima a localização que deseja consultar e tecle Enter</p>

      </div>

      <footer>
        <p>Projeto desenvolvido por <a href="https://linkedin.com/in/lucasgoulart92" target="_blank">Lucas Goulart</a>.</p>
      </footer>

</main>
  </div>
</template>

<script>
export default {
  name: 'App',
  data () {
    return {
      api_key: '348887cb37c0874cd7952e81fc5daa43',
      url_base: "https://api.openweathermap.org/data/2.5/",
      query: '',
      weather: {}
    }
  },
  methods: {
    async fetchWeather(e){
      if (e.key == "Enter") {
        let response = await fetch(`${this.url_base}weather?q=${this.query}&units=metric&APPID=${this.api_key}`);
        let json = await response.json();
        this.weather = await json;
      }
    },
    setResults (results) {
      this.weather = results;
    },
    dateBuilder() {
      let d = new Date();
      let months = ["Janeiro", "Fevereiro", "Março", "Abril", "Maio", "Junho", "Julho", "Agosto", "Setembro", "Outubro", "Novembro", "Dezembro"];
      let days = ["Domingo", "Segunda-feira", "Terça-feira", "Quarta-feira", "Quinta-feira", "Sexta-feira", "Sábado"];
      let day = days[d.getDay()];
      let date = d.getDate();
      let month = months[d.getMonth()];
      let year = d.getFullYear();
      return `${day}, ${date} de ${month} de ${year}`;
    },
  }
}
</script>

<style>

@import url('https://fonts.googleapis.com/css2?family=Nunito:wght@200;500&display=swap');

* {
  margin: 0;
  padding: 0;
  box-sizing: border-box;
}
body {
  font-family: 'Nunito', sans-serif;
}
#app {
  background-color: #1e2a54;
  background-size: cover;
  background-position: bottom;
  transition: 0.4s;
  background-image: url('./assets/bgs/clouds.jpeg');
}

#app.warm {
  background-color: rgb(110, 160, 63);
  background-image: url('./assets/bgs/warm.jpeg');
}

main {
  min-height: 100vh;
  padding: 0 25px;
  display: flex;
  align-items: center;
  justify-content: center;
  flex-direction: column;
  background-color: rgba(0, 0, 0, 0.3);
  transition: all 0.2s;
}
.search-box {
  width: 280px;
  margin-bottom: 30px;
}
.search-box p {
  color: #fff;
  text-align: center;
  margin: 20px 0;
  padding: 0 20px;
  font-size: 12px;
}
.search-box .search-bar {
  display: block;
  width: 100%;
  padding: 15px;
  
  color: #313131;
  font-size: 20px;
  appearance: none;
  border:none;
  outline: none;
  background: none;
  box-shadow: 0px 0px 8px rgba(0, 0, 0, 0.25);
  background-color: rgba(255, 255, 255, 0.9);
  border-radius: 3px;
  transition: 0.4s;
}
.location-box .location {
  color: #FFF;
  font-size: 40px;
  font-weight: 500;
  text-align: center;
  text-shadow: 1px 3px rgba(0, 0, 0, 0.25);
}
.location-box .date {
  color: #FFF;
  font-size: 14px;
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
  color: #FFF;
  font-size: 120px;
  font-weight: 900;
  text-shadow: 3px 3px rgba(0, 0, 0, 0.25);
  border-radius: 16px;
  margin: 20px 0px;
}
.weather-box .weather {
  color: #FFF;
  font-size: 48px;
  font-weight: 700;
  font-style: italic;
  text-shadow: 3px 6px rgba(0, 0, 0, 0.25);
}

footer {
  position: fixed;
  padding: 20px;
  background-color:rgba(0, 0, 0, 0.6);
  width: 100%;
  bottom: 0;
}
footer p {
  color: #fff;
}
footer p a {
  color: #fff;
  font-weight: 900;
  text-decoration: none;
  transition: all 0.4s;
}

footer p a:hover {
  color: orange;
}
</style>