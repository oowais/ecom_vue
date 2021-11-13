<template>
  <div id="wrapper">
    <nav class="navbar is-dark">
      <div class="navbar-brand">
        <router-link to="/" class="navbar-item">
          <strong>eCom</strong>
        </router-link>
        <a
          class="navbar-burger"
          aria-label="menu"
          aria-expanded="false"
          data-target="navbar-menu"
          @click="showMobileMenu = !showMobileMenu"
        >
          <span aria-hidden="true"></span>
          <span aria-hidden="true"></span>
          <span aria-hidden="true"></span>
        </a>
      </div>

      <div
        class="navbar-menu"
        id="navbar-menu"
        :class="{ 'is-active': showMobileMenu }"
      >
        <div class="navbar-end">
          <router-link to="/electronics" class="navbar-item"
            >Electronics</router-link
          >
          <router-link to="/jewelery" class="navbar-item">Jewelery</router-link>
          <router-link to="/mens-clothing" class="navbar-item"
            >Men's Clothing</router-link
          >
          <router-link to="/womens-clothing" class="navbar-item"
            >Women's Clothing</router-link
          >

          <div class="navbar-item">
            <div class="buttons">
              <router-link to="/log-in" class="button is-light"
                >Login</router-link
              >

              <router-link to="/cart" class="button is-success">
                <span class="icon"><i class="fas fa-shopping-cart"></i></span>
                <span>Cart ({{ cartTotalLength }})</span>
              </router-link>
            </div>
          </div>
        </div>
      </div>
    </nav>

    <section class="section">
      <router-view />
    </section>

    <footer class="footer">
      <p class="has-text-centered">Copyright &copy; 2021</p>
    </footer>
  </div>
</template>

<script>
export default {
  data() {
    return {
      showMobileMenu: false,
      cart: {
        items: [],
      },
    };
  },
  beforeCreate() {
    this.$store.commit("initializeStore");
  },
  mounted() {
    this.cart = this.$store.state.cart
  },
  computed: {
    cartTotalLength() {
      let totalLength = 0;

      for (let i = 0; i < this.cart.items.length; i++) {
        totalLength += this.cart.items[i].quantity;
      }
      return totalLength;
    },
  },
};
</script>


<style lang="scss">
@import "../node_modules/bulma";
[debug],
[debug] *:not(g):not(path) {
  color: hsla(210, 100%, 100%, 0.9) !important;
  background: hsla(210, 100%, 50%, 0.5) !important;
  outline: solid 0.25rem hsla(210, 100%, 100%, 0.5) !important;

  box-shadow: none !important;
  filter: none !important;
}
</style>
