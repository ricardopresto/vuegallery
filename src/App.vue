<template>
  <div id="app">
    <Menu :currentList="currentList" :nameArray="nameArray" @list-change="listChange($event)" />
    <div id="grid">
      <Thumb
        v-for="(image, index) in imageArray[currentList]"
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
            :firstImage="firstImage"
            :lastImage="lastImage"
            :aspect="aspect"
            @prev-image="prevImage"
            @next-image="nextImage"
          />
          <Preview
            class="preview"
            key="preview2"
            v-if="!preview1"
            :currentImage="currentImage"
            :firstImage="firstImage"
            :lastImage="lastImage"
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
import Menu from "./components/Menu.vue";
import { imageArray, nameArray } from "./components/imageList";

export default {
  name: "App",
  data() {
    return {
      imageList: [],
      aspectList: [],
      imageArray: imageArray,
      nameArray: nameArray,
      currentList: 0,
      currentListName: "",
      thumbSize: 200,
      preview: false,
      preview1: false,
      currentImage: undefined,
      currentImageIndex: undefined,
      aspect: undefined,
      direction: "next",
      firstImage: false,
      lastImage: false
    };
  },
  components: { Thumb, Preview, Menu },
  mounted() {
    this.imageList = imageArray[this.currentList];
    this.currentListName = nameArray[this.currentList];
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
      this.positionCheck();
    },
    nextImage() {
      if (!this.lastImage) {
        this.direction = "next";
        this.currentImageIndex++;
        this.currentImage = this.imageList[this.currentImageIndex];
        this.aspect = this.aspectList[this.currentImageIndex];
        this.preview1 = !this.preview1;
      }
      this.positionCheck();
    },
    prevImage() {
      if (!this.firstImage) {
        this.direction = "prev";
        this.currentImageIndex--;
        this.currentImage = this.imageList[this.currentImageIndex];
        this.aspect = this.aspectList[this.currentImageIndex];
        this.preview1 = !this.preview1;
      }
      this.positionCheck();
    },
    positionCheck() {
      this.currentImageIndex == 0
        ? (this.firstImage = true)
        : (this.firstImage = false);
      this.currentImageIndex == this.imageArray[this.currentList].length - 1
        ? (this.lastImage = true)
        : (this.lastImage = false);
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
    },
    listChange(e) {
      this.currentList = e;
      this.imageList = imageArray[this.currentList];
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