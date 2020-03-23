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
      <Preview :currentImage="currentImage" :aspect="aspect" />
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
      currentImage: undefined,
      aspect: undefined
    };
  },
  components: { Thumb, Preview },
  mounted() {
    this.imageList = imageList;
  },
  methods: {
    imageClick(imageObject) {
      this.currentImage = imageObject.image;
      this.aspect = imageObject.aspect;
      this.showImage = true;
      console.log(this.currentImage);
    },
    hideImage() {
      this.showImage = false;
    }
  }
};
</script>

<style>
* {
  margin: 0;
  padding: 0;
  box-sizing: border-box;
}
</style>

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
  width: 100vw;
  height: 100vh;
  top: 0;
  background: rgba(255, 159, 159, 0.4);
  display: flex;
  align-items: center;
  justify-content: center;
}
</style>>

