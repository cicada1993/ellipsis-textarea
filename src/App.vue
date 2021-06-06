<template>
  <div id="app">
    <ellipsis-textarea :text="text" :maxRows="2"></ellipsis-textarea>
  </div>
</template>

<script>
import EllipsisTextarea from "./components/ellipsistextarea";
export default {
  name: "App",
  components: {
    EllipsisTextarea,
  },
  data() {
    return {
      text: "一轮巨大的水淋淋的鲜红月亮从村庄东边暮色苍茫的原野上升起来时，村子里弥漫的烟雾愈加厚重，并且似乎都染上了月亮的那种凄艳的红色。",
    };
  },
};
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>
