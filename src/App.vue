<template>
  <div id="app">
    <div id="grid">
      <Thumb
        v-for="(image, index) in imageList"
        :index="index"
        :key="index"
        :image="image"
        :thumbSize="thumbSize"
        @image-click="imageClick($event)"
        @set-aspect="setAspect($event)"
      />
    </div>
    <div class="preview" v-if="currentPreview == 1" @click="hideImage">
      <Preview key="preview1" :currentImage="currentImage" :aspect="aspect" />
    </div>
    <div class="preview" v-if="currentPreview == 2" @click="hideImage">
      <Preview key="preview2" :currentImage="currentImage" :aspect="aspect" />
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
      aspects: [],
      thumbSize: 200,
      currentPreview: 0,
      currentImage: undefined,
      currentImageIndex: undefined,
      aspect: undefined
    };
  },
  components: { Thumb, Preview },
  mounted() {
    this.imageList = imageList;
    window.addEventListener("keydown", this.key);
  },
  methods: {
    imageClick(imageObject) {
      this.currentImage = imageObject.image;
      this.currentImageIndex = imageObject.index;
      this.aspect = this.aspects[this.currentImageIndex];
      this.currentPreview = 1;
    },
    nextImage() {
      if (this.currentPreview == 1) {
        this.currentPreview = 2;
        this.currentImageIndex++;
        this.currentImage = this.imageList[this.currentImageIndex];
        this.aspect = this.aspects[this.currentImageIndex];
      } else {
        this.currentPreview = 1;
        this.currentImageIndex++;
        this.currentImage = this.imageList[this.currentImageIndex];
        this.aspect = this.aspects[this.currentImageIndex];
      }
    },
    prevImage() {
      if (this.currentPreview == 1) {
        this.currentPreview = 2;
        this.currentImageIndex--;
        this.currentImage = this.imageList[this.currentImageIndex];
        this.aspect = this.aspects[this.currentImageIndex];
      } else {
        this.currentPreview = 1;
        this.currentImageIndex--;
        this.currentImage = this.imageList[this.currentImageIndex];
        this.aspect = this.aspects[this.currentImageIndex];
      }
    },
    hideImage() {
      this.currentPreview = 0;
    },
    key(event) {
      if (event.key == "ArrowRight") {
        this.nextImage();
      } else if (event.key == "ArrowLeft") {
        this.prevImage();
      }
    },
    setAspect(aspectObject) {
      this.aspects[aspectObject.index] = aspectObject.aspect;
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
.preview {
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

