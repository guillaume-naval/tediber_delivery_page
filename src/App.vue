<template>
  <div id="app">
    <AppHeader />
    <!-- BLOC SUIVI DE COMMANDE -->
    <div class="wrapper">
      <section class="order-tracking" v-if="order">
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
            <div class="blueline"></div>
            <div class="drawer">
              <p class="light">Suivi commande</p>
              <img :src="arrowDrawer" alt="arrow" @click="openDrawer" />
            </div>
            <!-- ETAPES DU SUIVI -->
            <transition name="slide-fade2">
              <div class="tracking" v-show="isHidden">
                <span class="tracking_steps">
                  <img src="./assets/packing.svg" alt="box" />
                  <span class="checkbox">
                    <img
                      class="boxonly"
                      src="./assets/unchecked.svg"
                      alt="box" />
                    <img
                      :class="{ taskNotDone: !order.packing }"
                      class="check"
                      src="./assets/checked.svg"
                      alt="check" /></span
                ></span>
                <img
                  src="./assets/arrow_side.svg"
                  alt="arrow side"
                  class="arrow_side"
                />
                <span class="tracking_steps">
                  <img src="./assets/shipping.svg" alt="box in truck" />
                  <span class="checkbox">
                    <img
                      class="boxonly"
                      src="./assets/unchecked.svg"
                      alt="box" />
                    <img
                      :class="{ taskNotDone: !order.shipping }"
                      class="check"
                      src="./assets/checked.svg"
                      alt="check"
                  /></span>
                </span>
                <img
                  src="./assets/arrow_side.svg"
                  alt="arrow side"
                  class="arrow_side"
                />

                <span class="tracking_steps">
                  <img src="./assets/intransit.svg" alt="truck" />
                  <span class="checkbox">
                    <img
                      class="boxonly"
                      src="./assets/unchecked.svg"
                      alt="box" />
                    <img
                      :class="{ taskNotDone: !order.intransit }"
                      class="check"
                      src="./assets/checked.svg"
                      alt="check"
                  /></span>
                </span>
                <img
                  src="./assets/arrow_side.svg"
                  alt="arrow side"
                  class="arrow_side"
                />
                <span class="tracking_steps">
                  <img src="./assets/delivered.svg" alt="box at door" />
                  <span class="checkbox">
                    <img
                      class="boxonly"
                      src="./assets/unchecked.svg"
                      alt="box" />
                    <img
                      :class="{ taskNotDone: !order.delivered }"
                      class="check"
                      src="./assets/checked.svg"
                      alt="check"
                  /></span>
                </span>
              </div>
            </transition>
            <div class="blueline"></div>
            <div class="drawer">
              <p class="light">Informations sur les retours</p>
              <img :src="arrow" alt="arrow" />
            </div>
            <div class="blueline"></div>
          </div>
          <!-- LES ARTICLES -->
          <div class="products">
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
      <section class="shipping" v-if="order">
        <div class="section_title">
          <h1><span>INFORMATIONS SUR LA LIVRAISON</span></h1>
          <img src="./assets/zigzag.svg" alt="zigzag" />
        </div>
        <div class="card_info shadow delivery">
          <div class="delivery_info">
            <p class="card_info_subtitles">ADRESSE DE COLLECTE</p>
            <p class="maj light">{{ order.shippingInfo.name }}</p>
            <p class="light">{{ order.shippingInfo.road }}</p>
            <p class="light">{{ order.shippingInfo.city }}</p>
            <p class="light">{{ order.shippingInfo.zipcode }}</p>
            <p class="light">{{ order.shippingInfo.country }}</p>
          </div>
          <div class="grey"></div>
          <div class="delivery_info">
            <p class="card_info_subtitles">VOS COORDONNÉES</p>
            <p class="light">{{ order.user.name }}</p>
            <p class="light">{{ order.user.phone }}</p>
          </div>
          <div class="grey"></div>
          <div class="delivery_info">
            <p class="card_info_subtitles">LIVRAISON ESTIMÉE</p>
            <p class="light">{{ order.shippingInfo.deliveryDate }}</p>
          </div>
          <div class="grey"></div>
          <div class="delivery_info">
            <p class="card_info_subtitles">MODE DE LIVRAISON</p>
            <p class="light">
              Livraison standard
              <span class="light" v-if="order.shippingInfo.name === 'domicile'"
                >à</span
              ><span v-else class="light">en</span>
              {{ order.shippingInfo.name }}
            </p>
          </div>
        </div>
      </section>
      <!-- INFORMATIONS DE PAIEMENT -->
      <section class="billing" v-if="order">
        <div class="section_title">
          <h1><span>INFORMATIONS DE PAIEMENT</span></h1>
          <img src="./assets/zigzag.svg" alt="zigzag" />
        </div>
        <div class="card_info shadow">
          <div class="payement" v-if="order.user.payement === 'MASTERCARD'">
            <img src="./assets/mastercard.svg" alt="mastercard_logo" />
            <p class="payement">{{ order.user.payement }}</p>
          </div>
          <div class="payement" v-else>
            <img src="./assets/visa.jpg" alt="visa_logo" />
            <p class="light">{{ order.user.payement }}</p>
          </div>
        </div>
      </section>
      <!-- TOTAL COMMANDE -->
      <section class="order_total" v-if="order">
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
            <p>TOTAL :</p>
            <p>
              {{ productsTotalPrice(order.products) + order.shippingPrice }}
            </p>
          </div>
        </div>
      </section>
      <!-- BESOIN D'AIDE -->
      <section class="help">
        <div class="section_title">
          <h1><span>BESOIN D'AIDE ?</span></h1>
          <img src="./assets/zigzag.svg" alt="zigzag" />
        </div>
        <div class="blueline"></div>
        <div class="drawer">
          <p class="light">FOIRE AUX QUESTIONS TEDIBER</p>
          <img :src="arrow" alt="arrow" />
        </div>
        <div class="blueline"></div>
        <div class="drawer">
          <p class="light">LA GARANTIE TEDIBER</p>
          <img :src="arrow" alt="arrow" />
        </div>
        <div class="blueline"></div>
        <div class="drawer">
          <p class="light">REPRISE DE L'ANCIENNE LITERIE</p>
          <img :src="arrow" alt="arrow" />
        </div>
        <div class="blueline"></div>
        <div class="drawer">
          <p class="light">COMMENT FAIRE UN RETOUR ?</p>
          <img :src="arrow" alt="arrow" />
        </div>
        <div class="blueline"></div>
      </section>
    </div>
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
      order: null,
      arrowDrawer: require("./assets/arrow_down.svg"),
      arrow: require("./assets/arrow_down.svg"),
      isHidden: false,
    };
  },
  created() {
    this.fetchData();
  },
  computed: {
    checkTask: function () {
      return {
        active: this.isActive && !this.error,
        "text-danger": this.error && this.error.type === "fatal",
      };
    },
  },
  methods: {
    fetchData() {
      this.infoOpened = false;
      fetch(
        "https://tediber-gn-default-rtdb.europe-west1.firebasedatabase.app/order.json"
      )
        .then(async (response) => {
          const data = await response.json();
          if (!response.ok) {
            const error = (data && data.message) || response.statusText;
            return Promise.reject(error);
          }

          this.order = data;
          console.log(this.order);
        })
        .catch((error) => {
          this.errorMessage = error;
          console.error("There was an error!", error);
        });
    },
    openDrawer() {
      this.isHidden = !this.isHidden;
      if (!this.isHidden) {
        this.arrowDrawer = require("./assets/arrow_down.svg");
      } else {
        this.arrowDrawer = require("./assets/arrow_up.svg");
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
a:hover {
  color: #f8ae00;
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
.drawer img:hover {
  filter: invert(74%) sepia(98%) saturate(2477%) hue-rotate(2deg)
    brightness(100%) contrast(100%);
}
.wrapper {
  @media screen and (min-width: 1200px) {
    padding: 0 80px;
  }
}
/* ANIMATIONS */

.slide-fade2-enter-active {
  transition: all 0.2s ease;
}
.slide-fade2-leave-active {
  transition: all 0.2s ease;
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
  @media screen and (min-width: 768px) {
    display: flex;
    & > * {
      flex-grow: 1;
      flex-basis: 50%;
    }
    .order_details {
      margin-right: 47px;
    }
  }
}

.tracking {
  display: flex;
  align-items: center;
  justify-content: center;
  margin: 20px 0;
  img {
    height: 40px;
    @media screen and (min-width: 1200px) {
      height: 60px;
    }
  }
  .arrow_side {
    width: 30px;
    margin: 0 10px 50px 10px;
  }
  &_steps {
    display: flex;
    flex-direction: column;
    justify-content: space-between;
    align-items: center;
    .checkbox {
      display: flex;
      justify-content: center;
      .check {
        width: 18px;
      }
      .taskNotDone {
        opacity: 0;
      }
      .boxonly {
        position: absolute;
        width: 14px;
      }
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
  padding: 18px;
  margin: 18px;
  &_subtitles {
    font-weight: bold;
    margin-bottom: 1em;
  }
  img {
    width: 53px;
  }
}
.grey {
  border-bottom: 2px solid rgba(0, 0, 0, 0.089);
  margin: 14px 0px;
}
.blueline {
  margin: 10px 0px;
  border-bottom: 1px solid #202447;
}
.shipping {
  .delivery {
    @media screen and (min-width: 1200px) {
      display: flex;
      justify-content: space-around;
      .grey {
        border-left: 3px solid rgba(0, 0, 0, 0.089);
        margin: 0;
      }
    }
  }
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
  @media screen and (min-width: 768px) {
    margin: 0 200px;
  }
  @media screen and (min-width: 1200px) {
    margin: 0 400px;
  }
}
.help {
  margin-bottom: 60px;
  .drawer {
    margin: 0 15px 0 33px;
    @media screen and (min-width: 1200px) {
      margin: 0 2px 0 6px;
    }
  }
}
</style>
