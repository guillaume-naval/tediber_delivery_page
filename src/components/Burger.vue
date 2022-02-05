<template>
  <div
    class="main-item menu"
    :class="{ close: isBurgerActive }"
    @click="toggle()"
  >
    <span class="line line01"></span>
    <span class="line line02"></span>
    <span class="line line03"></span>
  </div>
</template>

<script>
export default {
  name: "Burger",
  props: {
    isBurgerActive: {
      type: Boolean,
      required: true,
      default: false,
    },
  },
  methods: {
    toggle() {
      this.$emit("toggleMenu");
    },
  },
};
</script>
<style scoped lang="scss">
.main-item {
  display: flex;
  width: 26px;
  height: 30px;
  position: relative;
  cursor: pointer;
}
.line {
  position: absolute;
  height: 2px;
  width: 100%;
  background: black;
  transition: all cubic-bezier(0.25, 0.1, 0.28, 1.54) 0.32s;
}
.line01 {
  top: 19%;
}
.line02 {
  top: 49%;
}
.line03 {
  top: 79%;
}
.menu.close .line01 {
  transform: rotate(45deg);
  top: 49%;
}
.menu.close .line02,
.menu.close .line03 {
  transform: rotate(-45deg);
  top: 49%;
}
</style>
