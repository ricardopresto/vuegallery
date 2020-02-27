<template>
  <div id="app">
    <div id="grid">
      <Thumb
        v-for="image in imageList"
        :key="image"
        :image="image"
        :thumbSize="thumbSize"
        @image-click="imageClick($event)"
      />
    </div>
    <div id="preview" v-if="showImage == true" @click="hideImage">
      <Preview :currentImage="currentImage" />
    </div>
  </div>
</template>

<script>
import Thumb from "./components/Thumb.vue";
import Preview from "./components/Preview.vue";
import imageList from "./components/imageList";
export default {
  name: "App",
  data() {
    return {
      imageList: [],
      thumbSize: 200,
      showImage: false,
      currentImage: undefined
    };
  },
  components: { Thumb, Preview },
  mounted() {
    this.imageList = imageList;
  },
  methods: {
    imageClick(image) {
      this.currentImage = image;
      this.showImage = true;
      console.log(this.currentImage);
    },
    hideImage() {
      this.showImage = false;
    }
  }
};
</script>

<style scoped>
#app {
  position: relative;
}
#grid {
  display: grid;
  grid-template-columns: repeat(auto-fill, minmax(210px, 1fr));
  grid-gap: 5px;
}
#preview {
  position: absolute;
  width: 100%;
  height: 100%;
  top: 0;
  background: rgba(0, 0, 0, 0.4);
  display: flex;
  align-items: center;
  justify-content: center;
}
</style>>

