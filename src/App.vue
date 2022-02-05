<template>
  <div id="app">
    <AppHeader />
    <section id="order-tracking">
      <div class="section_title">
        <h1>SUIVI DE COMMANDE</h1>
        <img src="./assets/zigzag.svg" alt="zigzag" />
      </div>
    </section>
    <section id="shipping"></section>
    <section id="billing"></section>
  </div>
</template>

<script>
import AppHeader from "@/components/AppHeader.vue";

export default {
  name: "App",
  components: {
    AppHeader,
  },
};
</script>

<style lang="scss">
* {
  margin: 0;
  padding: 0;
  font-family: "Lato", sans-serif;
}
a {
  color: inherit;
  text-decoration: none;
  list-style: none;
}
h1 {
  letter-spacing: 3px;
  font-size: 22px;
}
</style>
