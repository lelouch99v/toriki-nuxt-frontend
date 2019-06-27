<template>
  <div>
    <div class="gacha-top">
      <div class="row justify-content-center">
        <form>
          <b-form-group>
            <div class="col-12">
              <span>食べもの：</span>
              <input type="number" v-model="numOfFood">
              <span>個</span>
            </div>
            <div class="col-12">
              <span>飲みもの：</span>
              <input type="number" v-model="numOfDrink">
              <span>個</span>
            </div>
          </b-form-group>
        </form>
      </div>
      <div class="btn-area">
        <button type="button" class="btn btn-primary btn-lg" @click="turnGacha">ガチャを回す</button>
      </div>
    </div>
    <div class="gacha-result" v-if="foodList.length>0 || drinkList.length>0">
      <div class="row">
        <div class="col-12">
          <h2>食べもの</h2>
        </div>
        <div class="col-xs-12 col-md-9 offset-md-3 text-left menu-list">
          <ul>
            <li v-for="food in foodList" :key="food.id">{{food.name}}</li>
          </ul>
        </div>
        <div class="col-12">
          <h2>飲みもの</h2>
        </div>
        <div class="col-xs-12 col-md-9 offset-md-3 text-left menu-list">
          <ul>
            <li v-for="drink in drinkList" :key="drink.id">{{drink.name}}</li>
          </ul>
        </div>
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
      numOfFood: 1,
      numOfDrink: 1,
      foodList: [],
      drinkList: []
    };
  },
  methods: {
    turnGacha: function(event) {
      axios
        .post(API_URL + "turn_gacha", {
          numOfFood: this.numOfFood,
          numOfDrink: this.numOfDrink
        })
        .then(res => {
          this.foodList = res.data.foodList;
          this.drinkList = res.data.drinkList;
        });
      this.$el.blur();
    }
  }
};
</script>

<style lang="scss">
div.gacha-top {
  font-size: 1.2rem;
  padding: 3em 0;
  div.btn-area {
    padding-top: 3em;
  }
}

.btn {
  font-weight: 600;
  background-color: #ae1e24;
  border: #6c1014 1px solid;
  &:hover {
    background-color: #96191d;
  }
  &:focus,
  &:active {
    background-color: #6c1014 !important;
    outline: none !important;
  }
}

div.gacha-result {
  h2 {
    padding: 0.5em 0;
  }
  div.menu-list {
    li {
      font-size: 1.5rem;
      padding: 0.1em 0;
    }
  }
}

input[type="number"] {
  text-align: center;
  width: 5em;
}
</style>
