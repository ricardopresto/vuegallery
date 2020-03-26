<template>
  <div id="container">
    <div id="menuBtn">
      <div class="caption">{{nameArray[currentList]}}</div>
      <div id="menuBox">
        <div
          v-for="(name, index) in nameArray"
          :key="index"
          class="menuItem"
          @click="menuClick(index)"
        >
          <div class="caption">{{name}}</div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: "Menu",
  props: ["currentList", "nameArray"],
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
  width: 210px;
  height: 30px;
  margin-right: 5px;
  border: 1px solid #d3d3d3;
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
  width: 210px;
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
  opacity: 1;
  background-color: #d3d3d3;
  display: flex;
  align-items: center;
  justify-content: center;
  transition: all 0.1s;
}
.menuItem:hover {
  background-color: #a0a0a0;
}
#caption {
  margin-left: 30px;
  margin-top: 5px;
}
</style>