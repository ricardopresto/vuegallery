<template>
  <div id="container">
    <img :src="filename" :width="imageWidth" :height="imageHeight" />
    <div
      id="overlay"
      @touchstart="touchS"
      @touchmove="touchM"
      @touchend="touchE"
    >
      <div
        id="overlayLeft"
        class="overlayEdge"
        v-if="!touchScreen"
        @click.stop="$emit('prev-image')"
      >
        <svg
          v-show="!firstImage"
          xmlns="http://www.w3.org/2000/svg"
          width="100"
          height="100"
          viewBox="0 0 24 24"
          fill="none"
          stroke="#fff"
          stroke-width="2"
          stroke-linecap="round"
          stroke-linejoin="round"
        >
          <path d="M15 18l-6-6 6-6" />
        </svg>
      </div>
      <div
        id="overlayRight"
        class="overlayEdge"
        v-if="!touchScreen"
        @click.stop="$emit('next-image')"
      >
        <svg
          v-show="!lastImage"
          xmlns="http://www.w3.org/2000/svg"
          width="100"
          height="100"
          viewBox="0 0 24 24"
          fill="none"
          stroke="#fff"
          stroke-width="2"
          stroke-linecap="round"
          stroke-linejoin="round"
        >
          <path d="M9 18l6-6-6-6" />
        </svg>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: "Preview",
  props: ["currentImage", "firstImage", "lastImage", "aspect", "touchScreen"],
  data() {
    return {
      imageWidth: undefined,
      imageHeight: undefined,
      touch1: undefined,
      touch2: undefined
    };
  },
  computed: {
    filename: function() {
      return require(`../assets/images/${this.currentImage}.jpg`);
    }
  },
  mounted() {
    let aspectScreen = window.innerWidth / window.innerHeight;
    if (this.aspect > aspectScreen) {
      this.imageWidth = window.innerWidth * 0.8;
    } else {
      this.imageHeight = window.innerHeight * 0.8;
    }
  },
  methods: {
    touchS(e) {
      this.touch1 = e.targetTouches[0].clientX;
    },
    touchM(e) {
      this.touch2 = e.targetTouches[0].clientX;
    },
    touchE() {
      if (this.touch2 > this.touch1) {
        this.$emit("prev-image");
      } else if (this.touch2 < this.touch1) {
        this.$emit("next-image");
      }
    }
  }
};
</script>

<style scoped>
#wrapper {
  display: flex;
  flex-direction: column;
  align-items: center;
}
#container {
  display: flex;
  align-items: center;
  justify-content: center;
  padding: 20px;
  background-color: #fff;
  border: 1px solid #aaa;
  border-radius: 6px;
  position: relative;
  box-shadow: 4px 4px 20px #888;
}
#overlay {
  width: 100%;
  height: 100%;
  position: absolute;
  display: flex;
  flex-direction: row;
  justify-content: space-between;
}
.overlayEdge {
  width: 30%;
  height: 100%;
  opacity: 0;
  display: flex;
  align-items: center;
}
.overlayEdge:hover {
  opacity: 0.6;
}
#overlayLeft {
  justify-content: flex-start;
}
#overlayRight {
  justify-content: flex-end;
}
@media screen and (max-width: 400px) {
  #container {
    padding: 5px;
  }
}
@media screen and (max-height: 400px) {
  #container {
    padding: 5px;
  }
}
</style>
