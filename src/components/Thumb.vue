<template>
  <div>
    <img v-bind:src="filename" />
  </div>
</template>

<script>
export default {
  name: "Thumb",
  props: ["image"],
  computed: {
    filename: function() {
      return require(`../assets/images/thumbs/${this.image}_th.jpg`);
    }
  }
};
</script>

<style scoped>
</style>