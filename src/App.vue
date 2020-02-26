<template>
  <div id="app">
    <div id="grid">
      <Thumb v-for="image in imageList" :key="image" :image="image" />
    </div>
  </div>
</template>

<script>
import Thumb from "./components/Thumb.vue";
import imageList from "./components/imageList";
export default {
  name: "App",
  data() {
    return {
      imageList: []
    };
  },
  components: { Thumb },
  mounted() {
    this.imageList = imageList;
  }
};
</script>

<style>
</style>
