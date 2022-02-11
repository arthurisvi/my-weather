<template>
  <!-- night theme -->
  <div
    v-if="backgroundWeather"
    id="app"
    style="
      background: #efeebc;
      background: linear-gradient(to bottom, #efeebc 0%, #61d0cf 100%);
    "
  >
    <main>
      <div id="title-app">
        <h1>My Weather</h1>
      </div>

      <div class="container-search">
        <input
          class="search"
          type="text"
          placeholder="Search the city..."
          v-model="searchWeather"
          @keypress="getWeatherUser"
        />
      </div>

      <Card :weathers="weathers"></Card>
      <Modal
        :messageError="messageError"
        v-show="showModal"
        @close="closeModal"
      />
    </main>
  </div>

  <!-- day theme -->
  <div
    v-else
    id="app"
    style="
      background: #040b3c;
      background: linear-gradient(to bottom, #040b3c 0%, #233072 100%);
    "
  >
    <main>
      <div id="title-app">
        <h1>My Weather</h1>
      </div>

      <div class="container-search">
        <input
          class="search"
          type="text"
          placeholder="Search the city..."
          v-model="searchWeather"
          @keypress="getWeatherUser"
        />
      </div>

      <Card :weathers="weathers"></Card>
      <Modal
        :messageError="messageError"
        v-show="showModal"
        @close="closeModal"
      />
    </main>
  </div>
</template>

<script>
import Card from "../src/components/Card.vue";
import Modal from "../src/components/Modal.vue";

export default {
  name: "App",
  components: {
    Card,
    Modal,
  },
  data() {
    return {
      apiKey: "2706f71121dfcf2babf85d9248ab0c08",
      baseUrl: "https://api.openweathermap.org/data/2.5/",
      searchWeather: "",
      weathers: [],
      showModal: false,
      messageError: "",
      backgroundWeather: null,
    };
  },
  mounted() {
    //set theme day or night
    this.backgroundWeather = this.getCurrentTime();
    this.getWeather();
  },
  methods: {
    async getWeather() {
      var cities = [
        "brasilia",
        "caruaru",
        "gameleira",
        "recife",
        "rio de janeiro",
      ];

      for (let i = 0; i < cities.length; i++) {
        fetch(
          `${this.baseUrl}weather?q=${cities[i]}&&units=metric&APPID=${this.apiKey}`
        )
          .then((res) => {
            return res.json();
          })
          .then(this.setWeathersResults);
      }
    },

    async getWeatherUser(e) {
      if (e.key === "Enter") {
        console.log("entrou");
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
      console.log(results);
      if (results.cod === 200) {
        const cityWeather = {
          id: results.id,
          name: results.name,
          country: results.sys.country,
          temp: this.formatTemp(results.main.temp),
          feelsLike: this.formatTemp(results.main.feels_like),
          temp_min: this.formatTemp(results.main.temp_min),
          temp_max: this.formatTemp(results.main.temp_max),
          humidity: results.main.humidity,
          main: results.weather[0].main,
          description: results.weather[0].description,
          icon: `http://openweathermap.org/img/wn/${results.weather[0].icon}@2x.png`,
        };

        //checking if the city already exists
        const cityIsExistent = this.verifyCityAlreadyExists(
          this.weathers,
          results
        );

        if (cityIsExistent) {
          this.showModal = !this.showModal;
          this.messageError = "You are already monitoring this city!";
        } else {
          //limiting slots to 5 cities
          if (this.weathers.length < 5) {
            this.weathers.push(cityWeather);
            this.searchWeather = "";
          } else {
            this.showModal = !this.showModal;
            this.messageError =
              "You can only track a maximum of 5 cities. Exclude any!";
          }
        }
      } else {
        this.showModal = !this.showModal;
        this.messageError = "City not found. Make sure you typed it correctly.";
      }
    },

    formatTemp(temp) {
      return temp.toString().split(".")[0];
    },

    verifyCityAlreadyExists(weathers, results) {
      return weathers.find((city) => city.id === results.id);
    },
    closeModal() {
      this.showModal = !this.showModal;
    },

    getCurrentTime() {
      var data = new Date().getHours() + ":" + new Date().getMinutes;

      var newData = data.split(":");

      if (newData[0] >= 5 && newData[0] <= 18) {
        return true;
      } else {
        return false;
      }
    },
  },
};
</script>

<style>
@import url("https://fonts.googleapis.com/css2?family=Poppins&display=swap");

* {
  padding: 0;
  margin: 0;
  box-sizing: border-box;
  font-family: "Poppins";
}

#app {
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
}

h1 {
  color: white;
  text-shadow: #696969 1px 1px 1px;
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

/* responsive  screen mobile */
@media screen and (max-width: 768px) {
  .container-search .search {
    width: 250px;
    height: 40px;
  }
}
</style>
