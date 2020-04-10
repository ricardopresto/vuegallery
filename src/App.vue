<template>
  <div id="app" @touchstart="touched">
    <Menu
      :currentList="currentList"
      :titles="titles"
      @list-change="listChange($event)"
      @title-toggle="titles = !titles"
    />
    <div id="flexLayout">
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
      <div id="previewBackground" v-if="preview" @click="hideImage">
        <transition :name="direction" mode="out-in">
          <Preview
            class="preview"
            key="preview1"
            v-if="preview1"
            :currentImage="currentImage"
            :firstImage="firstImage"
            :lastImage="lastImage"
            :aspect="aspect"
            :touchScreen="touchScreen"
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
            :touchScreen="touchScreen"
            @prev-image="prevImage"
            @next-image="nextImage"
          />
        </transition>
        <div id="titleContainer" v-if="titles">
          {{ titleArray[currentList][currentImageIndex] }}
        </div>
      </div>
    </transition>
  </div>
</template>

<script>
import Thumb from "./components/Thumb.vue";
import Preview from "./components/Preview.vue";
import Menu from "./components/Menu.vue";
import { imageArray, titleArray } from "./components/imageList";

export default {
  name: "App",
  data() {
    return {
      imageList: [],
      aspectList: [],
      imageArray: imageArray,
      titleArray: titleArray,
      currentList: 0,
      currentListName: "",
      thumbSize: 260,
      titles: true,
      preview: false,
      preview1: false,
      currentImage: undefined,
      currentImageIndex: undefined,
      aspect: undefined,
      direction: "next",
      firstImage: false,
      lastImage: false,
      touchScreen: false
    };
  },
  components: { Thumb, Preview, Menu },
  mounted() {
    this.imageList = imageArray[this.currentList];
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
    hideImage(e) {
      if (e.target.id == "previewBackground") {
        this.preview = false;
        this.preview1 = false;
      }
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
    },
    touched() {
      this.touchScreen = true;
    }
  }
};
</script>

<style>
* {
  margin: 0;
  padding: 0;
  box-sizing: border-box;
  font-family: "Gotu", sans-serif;
  font-weight: 400;
  font-style: normal;
}
div {
  border: 0px solid red;
}
</style>

<style scoped>
#app {
  position: relative;
  padding: 20px;
  background-color: #f4f4f4;
}
#flexLayout {
  display: flex;
  flex-flow: row wrap;
  justify-content: center;
  align-items: center;
}
#previewBackground {
  position: absolute;
  width: 100vw;
  height: 100vh;
  top: 0;
  left: 0;
  background: rgba(255, 255, 255, 0.6);
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: center;
  overflow: hidden;
  position: fixed;
}
#titleContainer {
  display: flex;
  justify-content: center;
  align-items: center;
  height: 30px;
  background-color: #444;
  color: #fff;
  padding: 5px 80px;
  margin-top: 10px;
  border-radius: 15px;
  max-width: 90%;
  box-shadow: 2px 2px 8px #222, inset -2px -2px 8px #222;
  font-size: 0.9rem;
}
.next-enter-active,
.prev-enter-active {
  transition: all 0.1s ease-out;
}
.next-leave-active,
.prev-leave-active {
  transition: all 0.1s ease-in;
}
.next-enter {
  opacity: 0;
  transform: translateX(200px);
}
.next-leave-to {
  opacity: 0;
  transform: translateX(-200px);
}
.prev-enter {
  opacity: 0;
  transform: translateX(-200px);
}
.prev-leave-to {
  opacity: 0;
  transform: translateX(200px);
}
.appear-enter-active {
  transition: all 0.1s ease-out;
}
.appear-enter {
  opacity: 0;
}
.appear-leave-active {
  transition: all 0.1s ease-in;
}
.appear-leave-to {
  opacity: 0;
}
@media screen and (max-width: 400px) {
  #titleContainer {
    padding: 5px 20px;
    min-width: 70%;
    height: min-content;
    font-size: 0.8rem;
  }
}
</style>
