<template>
  <div id="thumbBox">
    <img
      :src="filename"
      :width="thumbWidth"
      :height="thumbHeight"
      @click="imageClick"
      @load="onImageLoad"
    />
  </div>
</template>

<script>
export default {
  name: "Thumb",
  props: ["image", "index", "thumbSize"],
  computed: {
    filename: function() {
      return require(`../assets/images/thumbs/${this.image}_th.jpg`);
    }
  },
  data() {
    return {
      thumbWidth: undefined,
      thumbHeight: undefined,
      aspectRatio: undefined,
      originalHeight: undefined,
      originalWidth: undefined
    };
  },
  methods: {
    imageClick() {
      this.$emit("image-click", {
        image: this.image,
        index: this.index
      });
    },
    onImageLoad() {
      this.thumbWidth = undefined;
      this.thumbHeight = undefined;
      this.originalWidth = this.$el.firstChild.naturalWidth;
      this.originalHeight = this.$el.firstChild.naturalHeight;
      this.aspectRatio = this.originalWidth / this.originalHeight;

      this.aspectRatio > 1
        ? (this.thumbWidth = this.thumbSize)
        : (this.thumbHeight = this.thumbSize);
      this.$emit("set-aspect", { index: this.index, aspect: this.aspectRatio });
    }
  }
};
</script>

<style scoped>
#thumbBox {
  margin: 15px;
}
img {
  padding: 10px;
  background-color: white;
  border: 1px solid #aaa;
  border-radius: 4px;
  box-shadow: 1px 1px 8px #bbb;
}
img:hover {
  box-shadow: 3px 3px 8px #bbb;
}
</style>