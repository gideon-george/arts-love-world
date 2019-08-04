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
                      
                      <form method="POST" action="https://voguepay.com/pay/">
<input type="hidden" name="v_merchant_id" value="5453-0089880" />
<input type="hidden" name="memo" value="Arts Love World Oil on canvas paintings framed on wood" />
<input type="hidden" name="notify_url" value="www.artsloveworld.store/success" />
<input type="hidden" name="success_url" value="www.artsloveworld.store/success" />
<input type="hidden" name="fail_url" value="www.artsloveworld.store/failure" />
<input type="hidden" name="developer_code" value="5d2c0eb22f149" />
<input type="hidden" name="cur" value="EUR" />
<input type="hidden" name="item_1" :value="finalPrice" />
<input type="hidden" name="price_1" :value="finalPrice" />
<input type="hidden" name="description_1" value="" />
<input type="image" src="https://voguepay.com/images/buttons/Buynow.png" class="buynowimg" style="width: 100%;" />
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
  head() {
    return {
      script: [{ src: "//voguepay.com/js/voguepay.js" }]
    };
  },
  name: "checkout",

  data() {
    return {
      modalTitle: "Checkout",
      removeLabel: "Remove from cart",
      cartEmptyLabel: "Your cart is empty",
      closeLabel: "Close",
      isCheckoutSection: false
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
      } else {
        this.$store.commit("showCheckoutModal", false);
        this.$store.commit("showLoginModal", true);
      }
    },
    onPrevBtn() {
      this.isCheckoutSection = false;
    },
    closedFunction() {
      alert("window closed");
    },

    successFunction(transaction_id) {
      alert("Transaction was successful, Ref: " + transaction_id);
    },

    failedFunction(transaction_id) {
      alert("Transaction was not successful, Ref: " + transaction_id);
    }
  }
};
</script>

