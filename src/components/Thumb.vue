<template>
  <div id="thumbBox">
    <img :src="filename" :width="thumbWidth" :height="thumbHeight" @click="imageClick" />
  </div>
</template>

<script>
export default {
  name: "Thumb",
  props: ["image", "index", "thumbSize"],
  computed: {
    filename: function() {
      return require(`../assets/images/thumbs/${this.image}_th.jpg`);
    },
    originalWidth: function() {
      return this.$el.firstChild.naturalWidth;
    },
    originalHeight: function() {
      return this.$el.firstChild.naturalHeight;
    },
    aspectRatio: function() {
      return this.originalWidth / this.originalHeight;
    }
  },
  data() {
    return {
      thumbWidth: undefined,
      thumbHeight: undefined
    };
  },
  mounted() {
    if (this.originalWidth > this.originalHeight) {
      this.thumbWidth = this.thumbSize;
    } else {
      this.thumbHeight = this.thumbSize;
    }
    this.$emit("set-aspect", { index: this.index, aspect: this.aspectRatio });
  },
  methods: {
    imageClick() {
      this.$emit("image-click", {
        image: this.image,
        index: this.index
      });
    }
  }
};
</script>

<style scoped>
#thumbBox {
  display: flex;
  align-items: center;
  justify-content: center;
  border: 1px solid lightgrey;
  padding: 5px;
  margin: 5px;
}
</style>