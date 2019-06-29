<template>
  <div class="gacha-container">
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
        <transition name="fade">
          <div v-if="isShow" class="position-relative">
            <div class="arrow">
              <div class="fuwafuwa"></div>
            </div>
            <button type="button" class="btn btn-primary btn-lg" @click="turnGacha">ガチャを回す</button>
          </div>
        </transition>
      </div>
    </div>
    <div class="gacha-result" v-if="foodList.length>0 || drinkList.length>0">
      <div class="row">
        <div class="col-12">
          <h2>食べもの</h2>
        </div>
        <div class="col-xs-12 col-md-9 offset-md-3 text-left menu-list">
          <ul>
            <li v-for="food in foodList" :key="food.id">
              <fa-icon icon="spinner" class="fa-3x fa-pulse"/>
              {{food.name}}
            </li>
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
import Vue from "vue";
import axios from "axios";
import { library } from "@fortawesome/fontawesome-svg-core";
import { faSpinner } from "@fortawesome/free-solid-svg-icons";
import { FontAwesomeIcon } from "@fortawesome/vue-fontawesome";

library.add(faSpinner);

Vue.component("font-awesome-icon", FontAwesomeIcon);

const API_BASE_URL = process.env.apiBaseUrl;

export default {
  name: "gacha",
  data() {
    return {
      numOfFood: 1,
      numOfDrink: 1,
      foodList: [],
      drinkList: [],
      isShow: false
    };
  },
  mounted() {
    this.isShow = true;
  },
  methods: {
    turnGacha: function(event) {
      axios
        .post(API_BASE_URL + "turn_gacha", {
          numOfFood: this.numOfFood,
          numOfDrink: this.numOfDrink
        })
        .then(res => {
          this.foodList = res.data.foodList;
          this.drinkList = res.data.drinkList;
        });
      event.target.blur();
    }
  }
};
</script>

<style lang="scss">
div.gacha-container {
  width: 100%;
  div.gacha-top {
    font-size: 1.2rem;
    padding: 1.5em 0 2.5em 0;
    form div.col-12 {
      padding: 0.2em 0;
    }
    div.btn-area {
      width: 100%;
      padding-top: 3.5em;
    }
  }
}

.btn {
  font-weight: 600;
  background-color: #ae1e24;
  border: #6c1014 1px solid;
  box-shadow: 0 5px 5px 1px rgba(0, 0, 0, 0.12),
    0 3px 5px 2px rgba(0, 0, 0, 0.24);
  &:hover {
    background-color: #96191d;
    border: #6c1014 1px solid;
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

.fade-enter-active,
.fade-leave-active {
  transition: opacity 0.5s;
}
.fade-enter,
.fade-leave {
  opacity: 0;
}

.fuwafuwa {
  animation: fuwafuwa 3s infinite;
}

@keyframes fuwafuwa {
  0% {
    transform: translateY(0px);
  }
  50% {
    transform: translateY(10px);
  }
  100% {
    transform: translateY(0px);
  }
}

.arrow {
  position: absolute;
  top: -3.4em;
  left: 48.4%;
  div {
    height: 25px;
    width: 25px;
    background-color: #0099ff;
    position: relative;
    box-shadow: 0 5px 5px 2px rgba(0, 0, 0, 0.12),
      0 3px 5px 2px rgba(0, 0, 0, 0.1);
    &::after {
      content: "";
      width: 0;
      height: 0;
      border-style: solid;
      border-width: 25px 0 25px 25px;
      border-color: transparent transparent transparent #0099ff;
      position: absolute;
      top: 45%;
      left: 0%;
      transform: rotate(90deg);
      filter: drop-shadow(11px 1px 5px rgba(0, 0, 0, 0.4));
    }
  }
}
</style>
