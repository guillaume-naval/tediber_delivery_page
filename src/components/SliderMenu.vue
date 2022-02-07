<template>
  <div id="menuDrawer">
    <ul>
      <a href="#">NOS PRODUITS</a>
      <hr />
      <a href="#">LE CONCEPT</a>
      <hr />
      <a href="#">AVIS</a>
      <hr />
      <a href="#">PUB TV</a>
      <hr />
      <a href="#">MON COMPTE</a>
      <hr />
      <a href="#">MAG</a>
      <hr />
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
  z-index: 1;
  width: 100%;
  padding-bottom: 12px;
  hr {
    background: rgba(29, 29, 29, 0.13);
  }
  ul {
    margin-top: 1em;
    display: flex;
    flex-direction: column;
    a {
      margin: 1em;
    }
  }
  @media screen and (min-width: 1200px) {
    display: none;
  }
}
</style>
