<template>
  <div class="section">
    <div class="card is-clearfix columns">
      <div class="card-content column is-two-thirds">
        <div class="card-content__title">
          <div class="column card-content__text">
            <div class="container col-md-6">
              <h1 class="title">Mail Art</h1>

              <p>You may want to get your oil on canvas paintings framed on wood through the mail</p>

              <p>Our Mail Art package is coming soon for certain countries</p>

              <h3 class="title">Members will receive notification on the imminent commencement.</h3>
            </div>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>


<script>
import VmProducts from "@/components/Products";
import { getByTitle } from "@/assets/filters";

export default {
  components: { VmProducts },

  data() {
    return {
      id: "",
      noProductLabel: "No product found",
      productsFiltered: [],
      mailhead: ["S/N", "Title", "Quantity", "Add", "Remove", "Price"]
    };
  },
  computed: {
    isUserLogged() {
      return this.$store.getters.isUserLoggedIn;
    },

    products() {
      if (this.$store.state.userInfo.hasSearched) {
        return this.getProductByTitle();
      } else {
        return this.$store.state.products;
      }
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
    },
    getProductByTitle() {
      let listOfProducts = this.$store.state.products,
        titleSearched = this.$store.state.userInfo.productTitleSearched;

      return (this.productsFiltered = getByTitle(
        listOfProducts,
        titleSearched
      ));
    }
  }
};
</script>



<style lang="scss" scoped>
.card-content {
  padding: 15px 10px 15px 0;

  &__text {
    margin: 15px 0;
  }
  &__reviews {
    display: inline-block;
    width: 100%;
    margin-bottom: 10px;
  }
}
</style>

