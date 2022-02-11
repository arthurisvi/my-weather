<template>
  <div class="card-container">
    <div class="card" v-for="city in weathersLocal" :key="city.id">
      <div id="button-delete">
        <button @click="removeToCard(city)">X</button>
      </div>

      <div class="box-card">
        <div>
          <h4>{{ city.name }}, {{ city.country }}</h4>
        </div>
        <div class="img-weather">
          <img :src="city.icon" alt="weather" />
        </div>
        <div class="weather-info">{{ city.main }}</div>
        <div class="weather-info">
          <h1>{{ city.temp }}°</h1>
        </div>
        <div class="weather-info">Feels like: {{ city.feelsLike }}°</div>

        <div class="weather-icons">
          <img :src="humityIcon" alt="weather" />
          <img :src="tempMaxIcon" alt="weather" />
          <img :src="tempMinIcon" alt="weather" />
        </div>

        <div class="temp-status">
          <div>
            <p>{{ city.humidity }}%</p>
          </div>
          <div>
            <p>{{ city.temp_max }}°</p>
          </div>
          <div>
            <p>{{ city.temp_min }}°</p>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: "Card",
  props: {
    weathers: [],
  },
  data() {
    return {
      weathersLocal: this.weathers,
      humityIcon:
        "https://uploaddeimagens.com.br/images/003/718/584/original/humity.png?1644506513",
      tempMinIcon:
        "https://uploaddeimagens.com.br/images/003/718/589/full/down.png?1644506558",
      tempMaxIcon:
        "https://uploaddeimagens.com.br/images/003/718/592/full/up.png?1644506579",
    };
  },
  methods: {
    removeToCard(city) {
      let index = this.weathers.indexOf(city);
      this.weathersLocal.splice(index, 1);
    },
  },
};
</script>

<style>
.card-container {
  width: 80%;
  display: flex;
  flex-direction: row;
  align-items: center;
  justify-content: center;
  padding: 30px 0px;
}

.card {
  width: 200px;
  height: 400px;
  background: rgba(255, 255, 255, 0.6);
  box-shadow: 0px 4px 4px rgba(0, 0, 0, 0.25);
  border-radius: 10px;
  margin: 0px 10px;
}

.box-card {
  margin: auto;
  width: 80%;
  height: 80%;
}

.box-card h1 {
  color: black;
  font-size: 56px;
}

.box-card h4 {
  text-align: center;
}

.img-weather {
  margin: auto;
  width: 60%;
  height: 32.5%;
}

.img-weather img {
  width: 100%;
}

.weather-info {
  text-align: center;
  margin-bottom: 5px;
  font-size: 14px;
}

.temp-status,
.weather-icons {
  display: flex;
  flex-direction: row;
  justify-content: space-around;
}

.weather-icons {
  padding: 5px;
}

#button-delete {
  display: flex;
  justify-content: right;
  padding: 5px 15px;
}

#button-delete button {
  border: none;
  background: none;
  cursor: pointer;
}

.temp-status p {
  font-size: 16px;
}

.weather-icons img {
  width: 36px;
  height: 36px;
}

/* responsive  screen mobile */
@media screen and (max-width: 768px) {
  .card-container {
    padding-top: 1.5em;
    padding-bottom: 1.5em;
    display: flex;

    flex-direction: column;
    align-items: center;
    justify-content: center;
  }
  .card {
    flex: 0 1 60%;
    margin-bottom: 20px;
  }
}
</style>
