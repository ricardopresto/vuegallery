<template>
  <div id="container">
    <img :src="filename" :width="imageWidth" :height="imageHeight" />
  </div>
</template>

<script>
export default {
  name: "Preview",
  props: ["currentImage", "aspect"],
  data() {
    return {
      imageWidth: undefined,
      imageHeight: undefined
    };
  },
  computed: {
    filename: function() {
      return require(`../assets/images/${this.currentImage}.jpg`);
    }
  },
  mounted() {
    let aspectScreen = window.innerWidth / window.innerHeight;
    if (this.aspect > aspectScreen) {
      this.imageWidth = window.innerWidth * 0.9;
    } else {
      this.imageHeight = window.innerHeight * 0.9;
    }
  }
};
</script>

<style scoped>
</style>