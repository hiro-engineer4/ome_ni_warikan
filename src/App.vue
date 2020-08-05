<template>
  <div id="app">
    <div class="btn-wrap">
      <h1 class="btn btn-border-shadow title">
        <span class="text">多め ni 割り勘</span>
      </h1>
    </div>
    <Calculation />
  </div>
</template>

<script>
import Calculation from "./components/Calculation.vue";

export default {
  name: "App",
  components: {
    Calculation
  }
};
</script>

<style scoped>
#app {
  text-align: center;
  font-family: "Rounded Mplus 1c";
}

.title {
  margin: 20px;
  font-size: 2.1em;
}

@media screen and (max-width: 480px) {
  /* 480px以下に適用されるCSS（スマホ用） */
  .title {
    margin: 16px;
  }
}
</style>
