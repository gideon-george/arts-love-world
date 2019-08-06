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
                      
                      <p></p>
                      

                         <form
                  name="kyc"
                  action=" "
                  netlify-honeypot="bot-field"
                  method="post"
                  @change="formOnChange"
                  netlify
                >
                  <div class="form-group">
                    <label class="form-label" for="fullname">Full Name:</label>
                    <input class="form-field form-control " name="fullname" id="fullname" />
                  </div>
                  <div class="form-group">
                    <label class="form-label" for="email">Email:</label>
                    <input
                      class="form-field form-control form-control"
                      name="email"
                      id="email"
                    />
                  </div>
                   <div class="form-group">
                    <label class="form-label" for="phone">Phone:</label>
                    <input
                      class="form-field form-control form-control"
                      name="phone"
                      id="phone"
                    />
                  </div>

                  <div class="form-group">
                    <label class="form-label" for="city">City:</label>
                    <input
                      class="form-field form-control form-control"
                      name="city"
                      id="city"
                    />
                  </div>

                  <div class="form-group">
                    <label class="form-label" for="state">State:</label>
                    <input
                      class="form-field form-control form-control"
                      name="state"
                      id="state"
                    />
                  </div>

                  <div class="form-group">
                    <label class="form-label" for="country">Country:</label>
                    <input
                      class="form-field form-control form-control"
                      name="country"
                      id="country"
                    />
                  </div>


                  <div class="form-group">
                    <label class="form-label" for="shipping">Shipping Address:</label>
                    <textarea
                      class="form-field form-control"
                      rows="4"
                      name="shipping"
                      id="shipping"
                    >
                    </textarea>
                  </div>
                 <div class="form-group form-check">
    <input type="checkbox" class="form-check-input"  id="exampleCheck1">
    <label class="form-check-label" for="exampleCheck1">
      I accept the <a href="https://www.artsloveworld.store/terms">Terms / Conditions</a> 
      And <a href="https://www.artsloveworld.store/policy">Sales Policy</a>
    </label>
  </div>
    </form>

                      
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

