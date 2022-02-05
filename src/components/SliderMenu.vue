<template>
  <div id="menuDrawer">
    <ul>
      <a class="nav-link" href="#">NOS PRODUITS</a>
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
  background: rgb(255, 255, 255);
  top: 54px;
  z-index: 1;
  width: 100%;
  ul {
    margin-top: 1em;
    display: flex;
    flex-direction: column;
    a {
      margin: 1em;
    }
  }
}
</style>
