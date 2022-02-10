<template>
  <div class="card-container">
    <div class="card" v-for="city in weathersLocal" :key="city.id">
      <div>{{ city.name }}, {{ city.country }}</div>
      <div>Temp: {{ city.temp }} °C</div>
      <div>Max temp: {{ city.temp_max }} °C</div>
      <div>Min temp: {{ city.temp_min }} °C</div>
      <div>Humidity: {{ city.humidity }}</div>
      <div>Main: {{ city.main }}</div>
      <div>Description: {{ city.description }}</div>

      <button @click="removeToCard(city)">Excluir</button>
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
</style>
