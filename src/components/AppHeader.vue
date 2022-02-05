<template>
  <div id="header">
    <Burger
      @toggleMenu="menuActive = !menuActive"
      :isBurgerActive="menuActive"
    ></Burger>
    <transition name="slide-fade">
      <SliderMenu v-show="menuActive"></SliderMenu>
    </transition>
    <img src="../assets/logo_tediber.svg" alt="tediber logo" />
    <img src="../assets/cart.svg" alt="tediber logo" />
  </div>
</template>

<script>
import Burger from "@/components/Burger.vue";
import SliderMenu from "@/components/SliderMenu.vue";
export default {
  name: "AppHeader",
  components: {
    Burger,
    SliderMenu,
  },
  data() {
    return {
      menuActive: false,
    };
  },
};
</script>
<style scoped lang="scss">
#header {
  display: flex;
  justify-content: space-around;
  align-items: center;
  width: 100%;
  height: 54px;
  box-shadow: 0px 0px 5px rgba(0, 0, 0, 0.384);
}
.slide-fade-enter-active {
  transition: all 0.3s ease;
}
.slide-fade-leave-active {
  transition: all 0.8s ease;
}
.slide-fade-enter, .slide-fade-leave-to
/* .slide-fade-leave-active below version 2.1.8 */ {
  transform: translateX(-200px);
  opacity: 0;
}
</style>
