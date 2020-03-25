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
    <transition name="appear">
      <div class="previewBackground" v-if="preview" @click="hideImage">
        <transition :name="direction">
          <Preview
            class="preview"
            key="preview1"
            v-if="preview1"
            :currentImage="currentImage"
            :aspect="aspect"
            @prev-image="prevImage"
            @next-image="nextImage"
          />
          <Preview
            class="preview"
            key="preview2"
            v-if="!preview1"
            :currentImage="currentImage"
            :aspect="aspect"
            @prev-image="prevImage"
            @next-image="nextImage"
          />
        </transition>
      </div>
    </transition>
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
      aspectList: [],
      thumbSize: 200,
      preview: false,
      preview1: false,
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
      this.direction = "next";
      this.currentImage = imageObject.image;
      this.currentImageIndex = imageObject.index;
      this.aspect = this.aspectList[this.currentImageIndex];
      this.preview = true;
      this.preview1 = true;
    },
    nextImage() {
      if (this.currentImageIndex < this.imageList.length - 1) {
        this.direction = "next";
        this.currentImageIndex++;
        this.currentImage = this.imageList[this.currentImageIndex];
        this.aspect = this.aspectList[this.currentImageIndex];
        this.preview1 = !this.preview1;
      }
    },
    prevImage() {
      if (this.currentImageIndex > 0) {
        this.direction = "prev";
        this.currentImageIndex--;
        this.currentImage = this.imageList[this.currentImageIndex];
        this.aspect = this.aspectList[this.currentImageIndex];
        this.preview1 = !this.preview1;
      }
    },
    hideImage() {
      this.preview = false;
      this.preview1 = false;
    },
    key(event) {
      if (event.key == "ArrowRight") {
        this.nextImage();
      } else if (event.key == "ArrowLeft") {
        this.prevImage();
      }
    },
    setAspect(aspectObject) {
      this.aspectList[aspectObject.index] = aspectObject.aspect;
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
  background: rgba(255, 255, 255, 0.6);
  display: flex;
  align-items: center;
  justify-content: center;
  overflow: hidden;
  position: fixed;
}
.preview {
  position: absolute;
}
.next-enter-active,
.prev-enter-active {
  transition: all 0.2s ease-out;
}
.next-leave-active,
.prev-leave-active {
  transition: all 0.2s ease-in;
}
.next-enter {
  opacity: 0;
  transform: translateX(400px);
}
.next-leave-to {
  opacity: 0;
  transform: translateX(-400px);
}
.prev-enter {
  opacity: 0;
  transform: translateX(-400px);
}
.prev-leave-to {
  opacity: 0;
  transform: translateX(400px);
}
.appear-enter-active {
  transition: all 0.1s ease-out;
}
.appear-enter {
  opacity: 0;
}
.appear-leave-active {
  transition: all 0.1s ease-out;
}
.appear-leave-to {
  opacity: 0;
}
</style>