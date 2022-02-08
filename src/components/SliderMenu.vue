<template>
  <div id="menuDrawer">
    <ul>
      <a href="#">NOS PRODUITS</a>
      <a href="#">LE CONCEPT</a>
      <a href="#">AVIS</a>
      <a href="#">PUB TV</a>
      <a href="#">MON COMPTE</a>
      <a href="#">MAG</a>
      <a href="#">CONTACTEZ-NOUS</a>
    </ul>
  </div>
</template>

<script>
export default {
  name: "SliderMenu",
  props: {
    open: {
      type: Boolean,
      default: false,
    },
  },
};
</script>
<style scoped lang="scss">
#menuDrawer {
  font-size: 20px;
  position: absolute;
  background: rgb(240, 240, 240);
  box-shadow: 0px 5px 5px rgba(0, 0, 0, 0.13);
  top: 55px;
  z-index: 4;
  width: 100%;

  ul {
    display: flex;
    flex-direction: column;
    a {
      padding: 20px;
    }
    a:hover {
      background-color: rgba(214, 214, 214, 0.308);
    }
  }
  @media screen and (min-width: 1200px) {
    display: none;
  }
}
</style>
