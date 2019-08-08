<template>
    <div>

    
    <div class="table table-striped">
      <thead>
        <tr>
            <th scope="col" v-for="thead in mailhead" :key="thead.id"> {{thead}}</th>
        </thead>

        <tbody>
            <tr>
                <th scope="raw">{{product.id}}</th>
                <td>
            
                </td>
                <td>{{ product.title }}</td>

                <td>
                    <select @change="onSelectQuantity(product.id)" v-model="selected">
              <option v-for="quantity in quantityArray" :key="quantity.id" :value="quantity">{{ quantity }}</option>
                    </select>
                </td>

                <td><button class="button is-primary" v-if="!product.isAddedToCart" @click="addToCart(product.id)">{{ addToCartLabel }}</button></td>
                <td><button class="button is-text" v-if="product.isAddedToCart" @click="removeFromCart(product.id, false)">{{ removeFromCartLabel }}</button></td>

                <td>&euro;{{ product.price }}</td>
            </tr>
            <tr>
                <td>Shipping</td>
                <td>&euro;Shipping Price</td>
                
            </tr>

            <tr>
                <td>&euro;Total</td>
                <td>&euro;Total Price</td>
                <td>Checkout</td>
            </tr>
        </tbody>
        </div>
    </div>

</template>
    


<script>
export default {
  name: 'Mailorder',
  props: ['product'],

  data () {
    return {
      addToCartLabel: 'Add to cart',
      viewDetailsLabel: 'Details',
      removeFromCartLabel: 'Remove from cart',
      addToFavouriteLabel: 'Add to favourite',
      removeFromFavouriteLabel: 'Remove from favourite',
      selected: 1,
      quantityArray: [],
      mailhead:[ "S/N","Title", "Quantity", "Add", "Remove", "Price"]
    }
  },

  mounted () {
    for (let i = 1; i <= 20; i++) {
      this.quantityArray.push(i);
    }

    if (this.$props.product.quantity > 1) {
      this.selected = this.$props.product.quantity;
    }
  },

  computed: {
    isUserLogged () {
      return this.$store.getters.isUserLoggedIn;
    }
  },
  

  methods: {
    addToCart (id) {
      let data = {
        id: id,
        status: true
      }
      this.$store.commit('addToCart', id);
      this.$store.commit('setAddedBtn', data);
    },
    removeFromCart (id) {
      let data = {
        id: id,
        status: false
      }
      this.$store.commit('removeFromCart', id);
      this.$store.commit('setAddedBtn', data);
    },
    saveToFavorite (id) {
      let isUserLogged = this.$store.state.userInfo.isLoggedIn;

      if (isUserLogged) {
        this.$store.commit('addToFavourite', id);
      } else {
        this.$store.commit('showLoginModal', true);
      }
    },
    removeFromFavourite (id) {
      this.$store.commit('removeFromFavourite', id);
    },
    onSelectQuantity (id) {
      let data = {
        id: id,
        quantity: this.selected
      }
      this.$store.commit('quantity', data);
    }
  }
}
</script>

<style lang="scss" scoped>
 .details {
    cursor: pointer;
    position: absolute;
    top: 0;
    left: 0;
    width: 100%;
    height: 100%;
    z-index: 1;

    &:hover {
      border: 1px solid #51bafc;
    }
 }
 .button,
 .select {
   z-index: 2;
 }
 .select {
   position: absolute;
   right: 15px;
   bottom: 35px;
 }
 .card-content {
   padding: 0;
 }
 .buttons {
   margin: 0;
 }
</style>


