<script lang="ts">
import axios from "axios";

export default {
  data() {
    return {
      city: "",
      error: "",
      isVisible: false,
      info: null,
    };
  },
  computed: {
    cityName() {
      return this.city[0].toUpperCase() + this.city.slice(1);
    },
  },
  methods: {
    getWeather() {
      if (this.city.trim().length < 2) {
        this.error = "Нужно название более одного символа :)";
        return false;
      }
      this.error = "";
      axios
        .get(
          `https://api.openweathermap.org/data/2.5/weather?q=${this.city}&units=metric&appid=6fe91268b25ef7872146afe5f5c3adc0`
        )
        .then((res) => {
          this.info = res.data.main.temp + "-градусов";
        });
    },
    addBoxClassName() {
      if (this.city != "") {
        setTimeout(() => {
          this.isVisible = true;
        }, 0);
        return "show";
      } else {
        setTimeout(() => {
          this.isVisible = false;
        }, 300);
        return "hidden";
      }
    },
  },
};
</script>

<template>
  <div className="wrapper">
    <h1>Погодное приложение</h1>
    <p>
      Узнать погоду в
      <span>{{ city == "" ? "вашем городе" : cityName }}</span>
    </p>
    <input type="text" v-model="city" placeholder="Введите город" />
    <div :className="addBoxClassName()">
      <button v-if="isVisible" @click="getWeather()">Получить погоду</button>
    </div>
    <p className="error">{{ error }}</p>
    <p>{{ city == "" ? (info = null) : info }}</p>
  </div>
</template>

<style scoped>
* {
  font-family: cursive;
}
.hidden {
  visibility: hidden;
  display: flex;
  justify-content: center;
  height: 0;
  margin-top: 20px;
  transition: all 0.3s ease;
  overflow: hidden;
}
.show {
  visibility: visible;
  display: flex;
  justify-content: center;
  height: 43px;
  margin-top: 20px;
  transition: all 0.3s ease;
  overflow: hidden;
}
.error {
  color: #d03939;
}
.button-box {
  opacity: 0;
  margin-top: 20px;
}
.wrapper {
  padding: 20px;
  width: 900px;
  height: 500px;
  border-radius: 50px;
  background: #1f0f24;
  text-align: center;
  color: white;
}
.wrapper h1 {
  margin: 50px 0 0 0;
}
.wrapper p {
  margin: 20px 0 0 0;
}
.wrapper input {
  margin: 30px 0 0 0;
  padding: 5px 8px;
  font-size: 14px;
  background: transparent;
  border: 0;
  border-bottom: 2px solid #110813;
  color: #fcfcfc;
  outline: none;
  transition: all 0.3s ease;
}
.wrapper input:focus {
  border-bottom-color: #6e2d7d;
}

.wrapper button {
  margin: 0 0 0 20px;
  padding: 10px 15px;
  font-size: 14px;
  border-radius: 10px;
  border: 2px solid #b99935;
  background: #e3bc4b;
  color: #fff;
  cursor: pointer;
  transition: all 0.3s ease;
}
.wrapper button:hover {
  background: #ffce3d;
}
.wrapper button:active {
  background: #af8f2e;
}
</style>
