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
    <div class="previewBackground" v-if="preview" @click="hideImage">
      <transition name="next">
        <Preview key="preview1" v-if="preview1" :currentImage="currentImage" :aspect="aspect" />
      </transition>
    </div>
    <div class="previewBackground" v-if="preview" @click="hideImage">
      <transition name="next">
        <Preview key="preview2" v-if="!preview1" :currentImage="currentImage" :aspect="aspect" />
      </transition>
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
      preview: false,
      preview1: true,
      currentImage: undefined,
      currentImageIndex: undefined,
      aspect: undefined,
      direction: "next"
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
      this.preview = true;
    },
    nextImage() {
      this.currentImageIndex++;
      this.currentImage = this.imageList[this.currentImageIndex];
      this.aspect = this.aspects[this.currentImageIndex];
      this.preview1 = !this.preview1;
    },
    prevImage() {
      this.currentImageIndex--;
      this.currentImage = this.imageList[this.currentImageIndex];
      this.aspect = this.aspects[this.currentImageIndex];
      this.preview1 = !this.preview1;
    },
    hideImage() {
      this.currentPreview = 0;
      this.preview = false;
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
.previewBackground {
  position: absolute;
  width: 100vw;
  height: 100vh;
  top: 0;
  background: rgba(255, 159, 159, 0.4);
  display: flex;
  align-items: center;
  justify-content: center;
}
.next-enter-active,
.next.leave-active {
  transition: all 0.2s;
}
.next-enter {
  opacity: 0;
  transform: translatex(200px);
}
.next-leave-to {
  opacity: 0;
  transform: translatex(-200px);
}
</style>>

