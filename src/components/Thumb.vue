<template>
  <div id="thumbBox">
    <img :src="filename" :width="thumbWidth" :height="thumbHeight" @click="imageClick" />
  </div>
</template>

<script>
export default {
  name: "Thumb",
  props: ["image", "thumbSize"],
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
  },
  methods: {
    imageClick() {
      this.$emit("image-click", this.image);
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
}
</style>