<template>
  <div class="header">
    <Burger
      @toggleMenu="menuActive = !menuActive"
      :isBurgerActive="menuActive"
    ></Burger>
    <transition name="slide-fade">
      <SliderMenu v-show="menuActive"></SliderMenu>
    </transition>

    <img src="../assets/logo_tediber.svg" alt="tediber logo" />
    <div class="desktop_nav">
      <div class="side_nav">
        <a href="#"
          >NOS PRODUITS<img
            class="header_arrow"
            src="../assets/arrow_down.svg"
            alt="arrow"
        /></a>
        <a href="#">LE CONCEPT</a>
        <a href="#">AVIS</a>
      </div>
      <img
        class="tediber_head"
        src="../assets/tediber_head.svg"
        alt="tediber head"
      />
      <div class="side_nav">
        <a href="#">PUB TV</a>
        <a href="#">MON COMPTE</a>
        <a href="#">MAG</a>
        <a href="#">CONTACTEZ-NOUS</a>
      </div>
    </div>
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
/* ANIMATIONS */
.slide-fade-enter-active {
  transition: all 0.5s ease;
}
.slide-fade-leave-active {
  transition: all 0.3s ease;
}
.slide-fade-enter,
.slide-fade-leave-to {
  transform: translateX(-2000px);
  opacity: 0;
}

/* HEADER */
.header {
  display: flex;
  justify-content: space-around;
  align-items: center;
  width: 100%;
  height: 54px;
  box-shadow: 0px 0px 5px rgba(0, 0, 0, 0.384);
  a {
    font-family: "GillSansStd-light", sans-serif;
  }
  &_arrow {
    width: 12px;
    margin-left: 10px;
    filter: invert(86%) sepia(0%) saturate(0%) hue-rotate(205deg)
      brightness(97%) contrast(91%);
  }
  a:hover > &_arrow {
    filter: invert(74%) sepia(98%) saturate(2477%) hue-rotate(2deg)
      brightness(100%) contrast(100%);
  }
  .desktop_nav {
    display: none;
  }
  @media screen and (min-width: 1200px) {
    height: 80px;
    flex-grow: 2;
    .desktop_nav {
      justify-content: space-between;
      display: flex;
      width: 80%;
      align-items: center;
      .side_nav {
        display: flex;
        justify-content: space-evenly;
        flex-basis: 40%;
      }
    }
  }
}
</style>
