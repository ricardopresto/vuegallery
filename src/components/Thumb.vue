<template>
  <div>
    <img :src="filename" />
  </div>
</template>

<script>
export default {
  name: "Thumb",
  props: ["image"],
  computed: {
    filename: function() {
      return require(`../assets/images/thumbs/${this.image}_th.jpg`);
    },
    originalWidth: function() {
      return this.$el.firstChild.naturalWidth;
    },
    originalHeight: function() {
      return this.$el.firstChild.naturalHeight;
    }
  },
  mounted() {
    if (this.originalWidth > this.originalHeight) {
      console.log("landscape");
    } else {
      console.log("portrait");
    }
  }
};
</script>

<style scoped>
</style>