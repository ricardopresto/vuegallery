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
}
#menuBtn {
  width: 200px;
  height: 30px;
  margin-right: 20px;
  border: 1px solid #666;
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
  border: 1px solid #666;
  position: absolute;
  top: 28px;
  visibility: hidden;
  opacity: 0;
  transition: all 0.2s;
}
.menuItem {
  width: 100%;
  height: 30px;
  opacity: 1;
  background-color: #888;
  display: flex;
  align-items: center;
  justify-content: center;
}
.menuItem:hover {
  background-color: #555;
}
#caption {
  margin-left: 30px;
  margin-top: 5px;
}
</style>