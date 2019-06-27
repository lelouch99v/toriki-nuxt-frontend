<template>
  <div class="gacha-main">
    <div class="container">
      <gacha></gacha>
    </div>
  </div>
</template>

<script>
import Logo from "~/components/Logo.vue";
import Gacha from "~/components/Gacha.vue";

export default {
  components: {
    Logo,
    Gacha
  }
};
</script>

<style>
.container {
  margin: 0 auto;
  min-height: 100vh;
  display: flex;
  justify-content: center;
  text-align: center;
}
.gacha-main {
  background-image: url("./../assets/images/main_white.png");
  background-size: cover;
}
</style>
