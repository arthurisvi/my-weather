<template>
  <div class="card-container">
    <div class="card" v-for="city in weathersLocal" :key="city.id">
      <div class="box-card">
        <div>
          <h3>{{ city.name }}, {{ city.country }}</h3>
        </div>
        <div class="img-weather">img</div>
        <div class="weather-info">{{ city.main }}</div>
        <div class="weather-info">{{ city.temp }}°</div>
        <div class="weather-info">Feels like: {{ city.feelsLike }}°</div>

        <div class="temp-status">
          <div>{{ city.humidity }}%</div>
          <div>{{ city.temp_max }}°</div>
          <div>{{ city.temp_min }}°</div>
        </div>

        <button @click="removeToCard(city)">Excluir</button>
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
  padding: 30px 0px;
}

.card {
  width: 200px;
  height: 400px;
  background: #ffffff;
  box-shadow: 0px 4px 4px rgba(0, 0, 0, 0.25);
  border-radius: 10px;
  margin: 0px 10px;
}

.box-card {
  background: red;
  padding-top: 40px;
  margin: auto;
  width: 80%;
  height: 80%;
}

.box-card h3 {
  text-align: center;
  padding-bottom: 20px;
}

.img-weather {
  background: green;
  margin: auto;
  width: 60%;
  height: 40%;
}

.weather-info {
  text-align: center;
}

.temp-status div {
  display: inline-block;
}
</style>
