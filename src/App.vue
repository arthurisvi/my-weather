<template>
  <div id="app">
    <main>
      <div id="title-app">
        <h1>MY WEATHER</h1>
      </div>

      <div class="container-search">
        <input
          class="search"
          type="text"
          placeholder="Search the city..."
          v-model="searchWeather"
          @keypress="getWeather"
        />
      </div>

      <Card :weathers="weathers"></Card>
    </main>
  </div>
</template>

<script>
import Card from "../src/components/Card.vue";

export default {
  name: "App",
  components: {
    Card,
  },
  data() {
    return {
      apiKey: "2706f71121dfcf2babf85d9248ab0c08",
      baseUrl: "https://api.openweathermap.org/data/2.5/",
      searchWeather: "",
      weathers: [],
    };
  },
  methods: {
    async getWeather(e) {
      if (e.key === "Enter") {
        fetch(
          `${this.baseUrl}weather?q=${this.searchWeather}&&units=metric&APPID=${this.apiKey}`
        )
          .then((res) => {
            return res.json();
          })
          .then(this.setWeathersResults);
      }
    },

    setWeathersResults(results) {
      if (results.cod === 200) {
        const cityWeather = {
          id: results.id,
          name: results.name,
          country: results.sys.country,
          temp: results.main.temp,
          feelsLike: results.main.feels_like,
          temp_min: results.main.temp_min,
          temp_max: results.main.temp_max,
          humidity: results.main.humidity,
          main: results.weather[0].main,
          description: results.weather[0].description,
        };

        if (this.weathers.length < 5) {
          this.weathers.push(cityWeather);
          this.searchWeather = "";
        } else {
          alert("Você só pode acompanhar no máximo 5 cidades. Exclua alguma!");
        }
      } else {
        alert("Cidade não encontrada. Verifique se digitou corretamente.");
      }
    },
  },
};
</script>

<style>
* {
  padding: 0;
  margin: 0;
  box-sizing: border-box;
}

#app {
  background: blue;
  background-size: cover;
  background-position: bottom;
  transition: 0.4s;
}

main {
  width: 100%;
  display: flex;
  flex-direction: column;
  align-items: center;
  padding-top: 30px;
  min-height: 100vh;
  background-image: linear-gradient(
    to bottom,
    rgba(0, 0, 0, 0.25),
    rgb(0, 0, 0, 0.75)
  );
}

h1 {
  color: white;
}

.container-search .search {
  width: 400px;
  height: 40px;
  background: #ffffff;
  border: 1px solid #ffffff;
  box-shadow: 0px 4px 4px rgba(0, 0, 0, 0.25);
  border-radius: 0px 16px 0px 16px;
  margin: 10px 0px;
  padding: 10px;
  outline: none;
  appearance: none;
  transition: 0.4s;
}

.container-search .search:focus {
  box-shadow: 0px 0px 16px rgb(0, 0, 0, 0.25);
  border-radius: 16px 0px 16px 0px;
}
</style>
