<template>
  <div>
    <img :src="filename" :width="imageWidth" :height="imageHeight" />
  </div>
</template>

<script>
export default {
  name: "Preview",
  props: ["currentImage"],
  computed: {
    filename: function() {
      return require(`../assets/images/${this.currentImage}.jpg`);
    }
  },
  data() {
    return {
      imageWidth: 500,
      imageHeight: 400
    };
  }
};
</script>

<style scoped>
</style>