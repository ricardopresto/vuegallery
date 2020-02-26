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
    width: function() {
      return this.$el.firstChild.naturalWidth;
    },
    height: function() {
      return this.$el.firstChild.naturalHeight;
    }
  },
  mounted() {
    console.log(this.width, this.height);
  }
};
</script>

<style scoped>
</style>