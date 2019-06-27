<template>
  <div>
    <div class="gacha-header">
      <h1>鳥貴族ガチャ</h1>
    </div>
    <div class="gacha-body">
      <div class="row">
        <div class="col-12">
          <input v-model="numOfFood">
        </div>
        <div class="col-12">
          <input v-model="numOfDrink">
        </div>
      </div>
      <div>
        <button type="button" class="btn btn-primary" @click="turnGacha">ガチャを回す</button>
      </div>
    </div>
  </div>
</template>

<script>
import axios from "axios";

const API_URL = "http://localhost:5000/";

export default {
  name: "gacha",
  data() {
    return {
      numOfFood: 0,
      numOfDrink: 0,
      foodList: [],
      drinkList: []
    };
  },
  methods: {
    turnGacha: function() {
      axios
        .post(API_URL + "turn_gacha", {
          numOfFood: this.numOfFood,
          numOfDrink: this.numOfDrink
        })
        .then(res => {
          this.foodList = res.data.foodList;
          this.drinkList = res.data.drinkList;
        });
    }
  }
};
</script>

<style lang="scss">
div.gacha-body {
  .btn {
    &:focus {
      outline: none;
    }
  }
}
</style>
