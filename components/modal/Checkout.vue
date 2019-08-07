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
<<<<<<< HEAD
            <p>${{ product.price }}</p>
=======
            <p>&#36;{{ product.price }}</p>
>>>>>>> b7ebd4dc5dff7f741ce464cf1dc2df45cb3c89a5
          </div>
          <div v-if="products.length === 0">
            <p>{{ cartEmptyLabel }}</p>
                       
          </div>
        </div>
        <div v-if="isCheckoutSection">
<<<<<<< HEAD
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
=======
<p>Buy now and fill the form at the MailArt payment page, next...</p>
        <figure>
		<a href= "https://pay.gladepay.com/artslovemail">
        <img src="/img/buynow.png" alt="Buy now" />
		</a>
      </figure>
>>>>>>> b7ebd4dc5dff7f741ce464cf1dc2df45cb3c89a5
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
	name: 'checkout',
    
	data () {
		return {
			modalTitle: 'Checkout',
			removeLabel: 'Remove from cart',
			cartEmptyLabel: 'Your cart is empty',
			closeLabel: 'Close',
			isCheckoutSection: false
		}
	},

	computed: {
			products () {
				return this.$store.getters.productsAdded;
			},
			openModal () {
				if (this.$store.getters.isCheckoutModalOpen) {
					return true;
				} else {
					return false;
				}
			},
			buyLabel () {
				let totalProducts = this.products.length,
						productsAdded = this.$store.getters.productsAdded,
						pricesArray = [],
						productLabel = '',
						finalPrice = '',
						quantity = 1;

				productsAdded.forEach(product => {

					if (product.quantity >= 1) {
						quantity = product.quantity;
					}

					pricesArray.push((product.price * quantity)); // get the price of every product added and multiply quantity
				});

				finalPrice = pricesArray.reduce((a, b) => a + b, 0); // sum the prices
				
				if (totalProducts > 1) { // set plural or singular
					productLabel = 'products';
				} else {
					productLabel = 'product';
				}
				return `Buy ${totalProducts} ${productLabel} at $${finalPrice}`;
		},
		isUserLoggedIn () {
			return this.$store.getters.isUserLoggedIn;
		}
	},

	methods: {
		closeModal (reloadPage) {
			this.$store.commit('showCheckoutModal', false);

			if (reloadPage) {
				window.location.reload();
			}
		},
		removeFromCart (id) {
			let data = {
					id: id,
					status: false
			}
			this.$store.commit('removeFromCart', id);
			this.$store.commit('setAddedBtn', data);
		},
		onNextBtn () {
			if (this.isUserLoggedIn) {
				this.isCheckoutSection = true;
			} else {
				this.$store.commit('showCheckoutModal', false);
				this.$store.commit('showLoginModal', true);
			}
		},
		onPrevBtn () {
			this.isCheckoutSection = false;
		}
	}
}
</script>

