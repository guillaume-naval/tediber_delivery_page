<template>
  <div id="app">
    <AppHeader />
    <!-- BLOC SUIVI DE COMMANDE -->
    <section id="order-tracking" v-if="order">
      <div class="section_title">
        <h1><span>SUIVI DE COMMANDE</span></h1>
        <img src="./assets/zigzag.svg" alt="zigzag" />
      </div>
      <div class="order_status">
        <div class="order_details">
          <p class="details light">
            N° de commande : <span class="bold">{{ order.id }}</span>
          </p>
          <p class="details light">
            Date de commande : <span class="bold">{{ order.orderDate }}</span>
          </p>
          <p class="details light">
            Date d'expédition :
            <span class="bold">{{ order.shippingInfo.date }}</span>
          </p>
          <hr />
          <div class="drawer">
            <p class="light">Suivi commande</p>
            <img :src="url" alt="arrow" @click="openDrawer" />
          </div>
          <!-- ETAPES DU SUIVI -->
          <transition name="slide-fade2">
            <div class="tracking" v-show="isHidden">
              <span class="tracking_steps">
                <img src="./assets/Group.svg" alt="box" />
                <img class="checkbox" src="./assets/checked.svg" alt="checkbox"
              /></span>
              <img
                src="./assets/arrow_side.svg"
                alt="arrow side"
                class="arrow_side"
              />
              <span class="tracking_steps">
                <img src="./assets/Group2.svg" alt="box in truck" />
                <img
                  class="checkbox"
                  src="./assets/checked.svg"
                  alt="checkbox"
                />
              </span>
              <img
                src="./assets/arrow_side.svg"
                alt="arrow side"
                class="arrow_side"
              />

              <span class="tracking_steps">
                <img src="./assets/Group3.svg" alt="truck" />
                <img
                  class="checkbox"
                  src="./assets/checked.svg"
                  alt="checkbox"
                />
              </span>
              <img
                src="./assets/arrow_side.svg"
                alt="arrow side"
                class="arrow_side"
              />
              <span class="tracking_steps">
                <img src="./assets/Group4.svg" alt="box at door" />
                <img
                  class="checkbox"
                  src="./assets/checked.svg"
                  alt="checkbox"
                />
              </span>
            </div>
          </transition>
          <hr />
          <div class="drawer">
            <p class="light">Informations sur les retours</p>
            <img :src="url" alt="arrow" />
          </div>

          <hr />
        </div>
        <!-- LES ARTICLES -->
        <div id="products">
          <p class="article light">
            ARTICLES ({{ productsTotalQuantity(order.products) }})
          </p>
          <div
            class="products_list"
            v-for="product in order.products"
            :key="product.id"
          >
            <div class="products_card shadow">
              <img :src="product.url" alt="image produit" />
              <div class="products_card_text">
                <p class="bold">{{ product.name }}</p>
                <p class="bold">{{ product.price }} €</p>
                <p class="light">
                  TAILLE: {{ product.size }}<br />
                  QTÉ: {{ product.quantity }}
                </p>
              </div>
            </div>
          </div>
        </div>
      </div>
    </section>
    <!-- INFORMATIONS SUR LA LIVRAISON -->
    <section id="shipping" v-if="order">
      <div class="section_title">
        <h1><span>INFORMATIONS SUR LA LIVRAISON</span></h1>
        <img src="./assets/zigzag.svg" alt="zigzag" />
      </div>
      <div class="card_info shadow">
        <p class="card_info_subtitles">ADRESSE DE COLLECTE</p>

        <p class="maj light">{{ order.shippingInfo.name }}</p>
        <p class="light">{{ order.shippingInfo.road }}</p>
        <p class="light">{{ order.shippingInfo.city }}</p>
        <p class="light">{{ order.shippingInfo.zipcode }}</p>
        <hr class="grey" />
        <p class="card_info_subtitles">VOS COORDONNÉES</p>

        <p class="light">{{ order.user.name }}</p>
        <p class="light">{{ order.user.phone }}</p>
        <hr class="grey" />
        <p class="card_info_subtitles">LIVRAISON ESTIMÉE</p>

        <p class="light">{{ order.shippingInfo.deliveryDate }}</p>
        <hr class="grey" />
        <p class="card_info_subtitles">MODE DE LIVRAISON</p>

        <p class="light">
          Livraison standard
          <span class="light" v-if="order.shippingInfo.name === 'domicile'"
            >à</span
          ><span v-else class="light">en</span>
          {{ order.shippingInfo.name }}
        </p>
      </div>
    </section>
    <!-- INFORMATIONS DE PAIEMENT -->
    <section id="billing" v-if="order">
      <div class="section_title">
        <h1><span>INFORMATIONS DE PAIEMENT</span></h1>
        <img src="./assets/zigzag.svg" alt="zigzag" />
      </div>
      <div class="card_info shadow">
        <div class="payement" v-if="order.user.payement === 'MASTERCARD'">
          <img src="./assets/mastercard.png" alt="mastercard_logo" />
          <p class="payement">{{ order.user.payement }}</p>
        </div>
        <div class="payement" v-else>
          <img src="./assets/visa.jpg" alt="visa_logo" />
          <p class="light">{{ order.user.payement }}</p>
        </div>
      </div>
    </section>
    <!-- TOTAL COMMANDE -->
    <section id="total" v-if="order">
      <div class="section_title">
        <h1><span>TOTAL COMMANDE</span></h1>
        <img src="./assets/zigzag.svg" alt="zigzag" />
      </div>
      <div class="card_info shadow">
        <div class="total">
          <p class="light">Sous-totale</p>
          <p class="light">{{ productsTotalPrice(order.products) }}</p>
        </div>
        <div class="total">
          <p class="light">Livraison</p>
          <p class="light" v-if="order.shippingPrice === 0">GRATUITE</p>
          <p class="light" v-else>{{ order.shippingPrice }}</p>
        </div>
        <div class="total bold">
          <p>TOTAL</p>
          <p>{{ productsTotalPrice(order.products) + order.shippingPrice }}</p>
        </div>
      </div>
    </section>
    <!-- BESOIN D'AIDE -->
    <section id="help">
      <div class="section_title">
        <h1><span>BESOIN D'AIDE ?</span></h1>
        <img src="./assets/zigzag.svg" alt="zigzag" />
      </div>
      <hr />
      <div class="drawer">
        <p class="light">FOIRE AUX QUESTIONS TEDIBER</p>
        <img :src="url" alt="arrow" />
      </div>
      <hr />
      <div class="drawer">
        <p class="light">LA GARANTIE TEDIBER</p>
        <img :src="url" alt="arrow" />
      </div>
      <hr />
      <div class="drawer">
        <p class="light">REPRISE DE L'ANCIENNE LITERIE</p>
        <img :src="url" alt="arrow" />
      </div>
      <hr />
      <div class="drawer">
        <p class="light">COMMENT FAIRE UN RETOUR ?</p>
        <img :src="url" alt="arrow" />
      </div>
      <hr />
    </section>
  </div>
</template>

<script>
import AppHeader from "@/components/AppHeader.vue";

export default {
  name: "App",
  components: {
    AppHeader,
  },
  data() {
    return {
      order: {
        id: 40556003,
        orderDate: "1 mai 2019",

        shippingPrice: 0,
        shippingInfo: {
          name: "point relais",
          road: "57 rue Jean Pierre Timbaud",
          zipcode: 75011,
          city: "PARIS",
          deliveryDate: "9 mai 2019",
          date: "3 mai 2019",
        },
        products: [
          {
            id: 1,
            name: "L'INCROYABLE MATELAS TEDIBER",
            url: require("./assets/matelas.jpg"),
            price: 750,
            size: "160 x 200 cm",
            quantity: 1,
          },
          {
            id: 2,
            name: "L’INCROYABLE OREILLER TEDIBER",
            url: require("./assets/oreiller.jpg"),
            price: 85,
            size: "60 x 60 cm",
            quantity: 2,
          },
          {
            id: 3,
            name: "L'INCROYABLE COUETTE TEDIBER",
            url: require("./assets/couette.jpg"),
            price: 210,
            size: "240 x 220 cm",
            quantity: 1,
          },
        ],
        user: {
          name: "Shannon Honniball",
          phone: "0664262272",
          road: "64 rue de Paradis",
          zipCode: 75010,
          city: "PARIS",
          payement: "VISA",
        },
      },
      url: require("./assets/arrow_down.png"),
      url2: require("./assets/arrow_up.png"),
      isHidden: false,
    };
  },
  methods: {
    openDrawer() {
      this.isHidden = !this.isHidden;
      if (!this.isHidden) {
        this.url = require("./assets/arrow_down.png");
      } else {
        this.url = require("./assets/arrow_up.png");
      }
    },
    productsTotalQuantity(products) {
      var totalQuantity = 0;
      for (var i = 0; i < products.length; i++) {
        totalQuantity += products[i].quantity;
      }
      return totalQuantity;
    },
    productsTotalPrice(products) {
      var totalPrice = 0;
      for (var i = 0; i < products.length; i++) {
        totalPrice += products[i].price * products[i].quantity;
      }
      return totalPrice;
    },
  },
};
</script>

<style lang="scss">
@font-face {
  font-family: "GillSansStd-regular";
  src: local("GillSansStd-regular"),
    url("./assets/fonts/GillSansStd-regular.otf") format("opentype");
}
@font-face {
  font-family: "GillSansStd-light";
  src: local("GillSansStd-light"),
    url("./assets/fonts/GillSansStd-light.otf") format("opentype");
}

/* GLOBAL */
* {
  margin: 0;
  padding: 0;
  font-family: "GillSansStd-regular", sans-serif;
  box-sizing: border-box;
  color: #202447;
}

a {
  color: inherit;
  text-decoration: none;
  list-style: none;
}
.bold {
  font-family: "GillSansStd-regular", sans-serif;
  font-weight: 500;
}
.light {
  font-family: "GillSansStd-light", sans-serif;
}
.maj {
  text-transform: uppercase;
}
h1 {
  letter-spacing: 3px;
  font-size: 22px;
  display: flex;
  width: 100%;
  align-items: center;
  justify-content: center;
  margin: 40px 0 20px 0;
  span {
    text-align: center;
  }
}
h1:after,
h1:before {
  content: "";
  width: 20%;
  height: 2px;
  background: black;
  margin: 1%;
}
h3 {
  margin-left: 1em;
}
.section_title {
  display: flex;
  flex-direction: column;
  justify-content: center;
  align-items: center;
  margin-bottom: 30px;
}
.shadow {
  box-shadow: 0px 3px 9px rgba(0, 0, 0, 0.296);
}
.drawer {
  display: flex;
  justify-content: space-between;
  align-items: center;
  img {
    width: auto;
    height: 100%;
    cursor: pointer;
  }
}

/* ANIMATIONS */
.slide-fade-enter-active {
  transition: all 0.3s ease;
}
.slide-fade-leave-active {
  transition: all 0.8s ease;
}
.slide-fade-enter,
.slide-fade-leave-to {
  transform: translateX(-200px);
  opacity: 0;
}

.slide-fade2-enter-active {
  transition: all 0.3s ease;
}
.slide-fade2-leave-active {
  transition: all 0.3s ease;
}
.slide-fade2-enter,
.slide-fade2-leave-to {
  transform: translateY(-20px);
  opacity: 0;
}

/* SUIVI DE COMMANDE */
.order_status {
  padding: 0 19px;
  p {
    font-size: 16px;
  }
  .details {
    margin-bottom: 15px;
  }
}

.tracking {
  display: flex;
  align-items: center;
  justify-content: center;
  max-width: 50;
  flex-wrap: wrap;
  margin: 20px 0;
  img {
    width: auto;
    height: 40px;
  }
  .arrow_side {
    width: 25px;
    height: auto;
    margin: 0 10px 50px 10px;
  }
  &_steps {
    display: flex;
    flex-direction: column;
    justify-content: space-between;
    align-items: center;
    .checkbox {
      width: 15px;
    }
  }
}
.products {
  &_list {
    padding: 9.5px 0;
  }
  &_card {
    display: flex;
    padding: 11px;
    height: 131px;
    img {
      height: 103px;
      margin-right: 1em;
    }
    &_text {
      display: flex;

      flex-direction: column;
      justify-content: space-between;
      line-height: 1;
    }
  }
}

/* Card CSS */
.card_info {
  padding: 1em;
  margin: 1em;
  &_subtitles {
    font-weight: bold;
    margin-bottom: 1em;
  }
  img {
    width: 53px;
  }
}
.grey {
  background: #20244752;
}
hr {
  border: 0;
  margin: 10px 0px;
  width: 100%;
  height: 1px;
  background: #202447;
}

.payement {
  display: flex;
  align-items: center;
  p {
    margin-left: 10px;
  }
}
.total {
  display: flex;
  justify-content: space-between;
  align-items: center;
  height: 28px;
}
#help {
  margin-bottom: 60px;
  .drawer {
    margin: 0 15px 0 33px;
  }
}
</style>
