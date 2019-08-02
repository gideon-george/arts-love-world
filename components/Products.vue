<template>
  <div class="section">
    <div class="card is-clearfix columns">
      <div class="card-content column is-two-thirds">
        <div class="card-content__title">
          <div class="column card-content__text">
            <div class="container col-md">
              
              <div class="content col-md">
                <table class="table">
  <thead class="thead-light">
    <tr>
      <th scope="col"></th>
      <th scope="col">Product Title</th>
      <th scope="col">Product Price</th>
      <th scope="col">Add</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th scope="row"></th>
      <td>{{ product.title }}</td>
      <td>${{ product.price }}</td>
      <td>  
    <div class="btn-actions">
      <div>
        <div class="buttons">
          <button
            class="button is-primary"
            v-if="!product.isAddedToCart"
            @click="addToCart(product.id)"
          >{{ addToCartLabel }}</button>
          <button
            class="button is-text"
            v-if="product.isAddedToCart"
            @click="removeFromCart(product.id, false)"
          >{{ removeFromCartLabel }}</button>
        </div>
        
      </div>
    </div></td>
    </tr>
    
  </tbody>
</table>
              </div>
            </div>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: "products",
  props: ["product"],

  data() {
    return {
      addToCartLabel: "Add to cart",
      viewDetailsLabel: "Details",
      removeFromCartLabel: "Remove from cart",
      addToFavouriteLabel: "Add to favourite",
      removeFromFavouriteLabel: "Remove from favourite",
      selected: 1,
      quantityArray: []
    };
  },

  mounted() {
    for (let i = 1; i <= 20; i++) {
      this.quantityArray.push(i);
    }

    if (this.$props.product.quantity > 1) {
      this.selected = this.$props.product.quantity;
    }
  },

  computed: {
    isUserLogged() {
      return this.$store.getters.isUserLoggedIn;
    }
  },

  methods: {
    addToCart(id) {
      let data = {
        id: id,
        status: true
      };
      this.$store.commit("addToCart", id);
      this.$store.commit("setAddedBtn", data);
    },
    removeFromCart(id) {
      let data = {
        id: id,
        status: false
      };
      this.$store.commit("removeFromCart", id);
      this.$store.commit("setAddedBtn", data);
    },
    saveToFavorite(id) {
      let isUserLogged = this.$store.state.userInfo.isLoggedIn;

      if (isUserLogged) {
        this.$store.commit("addToFavourite", id);
      } else {
        this.$store.commit("showLoginModal", true);
      }
    },
    removeFromFavourite(id) {
      this.$store.commit("removeFromFavourite", id);
    },
    onSelectQuantity(id) {
      let data = {
        id: id,
        quantity: this.selected
      };
      this.$store.commit("quantity", data);
    }
  }
};
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


