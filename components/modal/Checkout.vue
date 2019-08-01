<template>
  <div :class="[ openModal ? 'is-active' : '', 'modal' ]">
    <div class="modal-background"></div>
    <div class="modal-card">
      <header class="modal-card-head">
        <p class="modal-card-title">{{ modalTitle }}</p>
        <button class="delete" aria-label="close" @click="closeModal(false)"></button>
      </header>
      <section class="modal-card-body">
        <div v-if="!isCheckoutSection">
          <div class="box" v-for="product in products" :key="product.id">
            <button
              class="is-pulled-right button is-info is-inverted"
              @click="removeFromCart(product.id)"
            >{{ removeLabel }}</button>

            <p>{{ product.title }} {{ product.quantity > 0 ? ` - Quantity: ${product.quantity}` : ''}}</p>
            <p>&euro;{{ product.price }}</p>
          </div>
          <div v-if="products.length === 0">
            <p>{{ cartEmptyLabel }}</p>
          </div>
        </div>
        <div v-if="isCheckoutSection">
          <div class="section">
            <div class="card is-clearfix columns">
              <div class="card-content column is-two-thirds">
                <div class="card-content__title">
                  <div class="column card-content__text">
                    <div class="container col-md-6">
                      <h6 class="title">Free Shipping</h6>
                      <p>For your oil on canvas paintings, framed on 27.6 x 19.7 size wood.</p>
                      <p></p>

                      <form
                        id="payform"
                        onsubmit="return false;"
                        method="POST"
                        action="https://voguepay.com/pay/"
                      >
                        <input type="hidden" name="v_merchant_id" value="qa331322179752" />
                        <input type="hidden" name="merchant_ref" value="234-567-890" />
                        <input
                          type="hidden"
                          name="memo"
                          value="Order for oil on canvas painting, framed on wood"
                        />

                        <input type="hidden" name="item_1" value="Face Cap" />
                        <input type="hidden" name="description_1" value="Blue Zizi facecap" />
                        <input type="hidden" :name="productLabel" :value="finalPrice" />

                        <input type="hidden" name="developer_code" value="5d2c0eb22f149" />
                        <input type="hidden" name="store_id" value="25" />

                        <input type="hidden" name="total" :value="finalPrice" />

                        <input type="hidden" name="cname" :value="cname" />
                        <input type="hidden" name="address" :value="address" />
                        <input type="hidden" name="city" :value="city" />
                        <input type="hidden" name="state" :value="state" />
                        <input type="hidden" name="zipcode" :value="zipcode" />
                        <input type="hidden" name="email" :value="email" />
                        <input type="hidden" name="phone" :value="phone " />
                        <input
                          type="image"
                          src="http://voguepay.com/images/buttons/buynow_blue.png"
                          alt="Submit"
                        />
                      </form>
                    </div>
                  </div>
                </div>
              </div>
            </div>
          </div>
        </div>
      </section>
      <footer class="modal-card-foot">
        <button
          v-show="products.length > 0 && !isCheckoutSection"
          class="button is-success"
          @click="onNextBtn"
        >{{ buyLabel }}</button>
        <button
          v-if="isCheckoutSection"
          class="button is-success"
          @click="closeModal(true)"
        >{{ closeLabel }}</button>
      </footer>
    </div>
  </div>
</template>

<script>
export default {
  name: "checkout",

  data() {
    return {
      modalTitle: "Checkout",
      removeLabel: "Remove from cart",
      cartEmptyLabel: "Your cart is empty",
      closeLabel: "Close",
      isCheckoutSection: false,
      address: "",
      city: "",
      state: "",
      zipcode: "",
      email: "",
      phone: ""
    };
  },

  computed: {
    products() {
      return this.$store.getters.productsAdded;
    },
    openModal() {
      if (this.$store.getters.isCheckoutModalOpen) {
        return true;
      } else {
        return false;
      }
    },
    buyLabel() {
      let totalProducts = this.products.length,
        productsAdded = this.$store.getters.productsAdded,
        pricesArray = [],
        productLabel = "",
        finalPrice = "",
        quantity = 1;

      productsAdded.forEach(product => {
        if (product.quantity >= 1) {
          quantity = product.quantity;
        }

        pricesArray.push(product.price * quantity); // get the price of every product added and multiply quantity
      });

      finalPrice = pricesArray.reduce((a, b) => a + b, 0); // sum the prices

      if (totalProducts > 1) {
        // set plural or singular
        productLabel = "products";
      } else {
        productLabel = "product";
      }
      return `Buy ${totalProducts} ${productLabel} at €${finalPrice}`;
    },
    isUserLoggedIn() {
      return this.$store.getters.isUserLoggedIn;
    }
  },

  methods: {
    closeModal(reloadPage) {
      this.$store.commit("showCheckoutModal", false);

      if (reloadPage) {
        window.location.reload();
      }
    },
    removeFromCart(id) {
      let data = {
        id: id,
        status: false
      };
      this.$store.commit("removeFromCart", id);
      this.$store.commit("setAddedBtn", data);
    },
    onNextBtn() {
      if (this.isUserLoggedIn) {
        this.isCheckoutSection = true;
        Voguepay.init({ form: "payform" }, { cur: "EUR" });
      } else {
        this.$store.commit("showCheckoutModal", false);
        this.$store.commit("showLoginModal", true);
      }
    },
    onPrevBtn() {
      this.isCheckoutSection = false;
    }
  }
};
</script>

