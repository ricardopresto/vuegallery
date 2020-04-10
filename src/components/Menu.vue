<template>
  <div id="container">
    <div id="menuBtn">
      <div class="caption" v-if="currentList == 0">Black & White</div>
      <div class="caption" v-if="currentList == 1">Colour</div>
      <div id="menuBox">
        <div v-if="currentList == 0" class="menuItem" @click="menuClick(1)">
          <div class="caption">Colour</div>
        </div>
        <div v-if="currentList == 1" class="menuItem" @click="menuClick(0)">
          <div class="caption">Black & White</div>
        </div>
        <div id="titleOption" class="menuItem" @click="$emit('title-toggle')">
          <span>Titles</span>
          <span>
            <svg
              v-show="titles"
              xmlns="http://www.w3.org/2000/svg"
              width="20"
              height="20"
              viewBox="0 0 24 24"
              fill="none"
              stroke="#0f0"
              stroke-width="2.5"
              stroke-linecap="round"
              stroke-linejoin="round"
            >
              <polyline points="22 8 11 19 6 14" />
            </svg>
            <svg
              v-show="!titles"
              xmlns="http://www.w3.org/2000/svg"
              width="20"
              height="20"
              viewBox="0 0 24 24"
              fill="none"
              stroke="#ff0000"
              stroke-width="2.5"
              stroke-linecap="round"
              stroke-linejoin="round"
            >
              <line x1="20" y1="8" x2="8" y2="20" />
              <line x1="8" y1="8" x2="20" y2="20" />
            </svg>
          </span>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: "Menu",
  props: ["currentList", "titles"],
  methods: {
    menuClick(e) {
      this.$emit("list-change", e);
    }
  }
};
</script>

<style scoped>
#container {
  width: 100%;
  height: 50px;
  display: flex;
  flex-direction: row;
  align-items: center;
  justify-content: flex-end;
  cursor: default;
  user-select: none;
}
#menuBtn {
  width: 200px;
  height: 30px;
  margin-right: 10px;
  border: 1px solid #aaa;
  background-color: #fff;
  box-shadow: 2px 2px 8px #aaa, inset -2px -2px 8px #aaa;
  border-radius: 15px;
  position: relative;
  display: flex;
  align-items: center;
  justify-content: center;
}
#menuBtn:hover #menuBox {
  visibility: visible;
  opacity: 1;
}
#menuBox {
  width: 200px;
  height: min-content;
  position: absolute;
  top: 28px;
  visibility: hidden;
  opacity: 0;
  transition: all 0.1s;
}
.menuItem {
  width: 100%;
  height: 30px;
  border: 1px solid #aaa;
  box-shadow: 1px 1px 4px #aaa, inset -1px -1px 4px #aaa;
  border-radius: 15px;
  opacity: 0.8;
  background-color: #fff;
  display: flex;
  align-items: center;
  justify-content: center;
  transition: all 0.1s;
}
.menuItem:hover {
  box-shadow: 1px 1px 4px #aaa, inset -2px -2px 6px #aaa;
  background-color: #ddd;
}
#caption {
  margin-left: 30px;
  margin-top: 5px;
}
@media screen and (max-width: 400px) {
  #container {
    justify-content: center;
  }
  #menuBtn {
    margin-right: 0;
  }
}
</style>
