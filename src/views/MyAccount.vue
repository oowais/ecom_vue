<template>
  <div class="page-my-account">
    <div class="columns is-multiline">
      <div class="column is-12">
        <h1 class="title">My account</h1>
      </div>
      <div class="column is-12">
        <button class="button is-danger" @click="logout()">Log out</button>
      </div>

      <div class="column is-12">
        <h2 class="subtitle">My Orders</h2>
        <OrderSummary v-for="order in orders" :key="order.id" :order="order" />
      </div>
    </div>
  </div>
</template>

<script>
import axios from "axios";

import OrderSummary from "@/components/OrderSummary.vue";

export default {
  name: "MyAccount",
  components: {
    OrderSummary,
  },
  data() {
    return {
      orders: [],
    };
  },
  mounted() {
    document.title = "My Orders | eCom";

    this.getMyOrders();
  },
  methods: {
    logout() {
      axios.defaults.headers.common["Authoization"] = "";

      localStorage.removeItem("token");
      localStorage.removeItem("username");
      localStorage.removeItem("userid");

      this.$store.commit("removeToken");

      this.$router.push("/");
    },
    async getMyOrders() {
      this.$store.commit("setIsLoading", true);
      await axios
        .get("/api/v1/orders/")
        .then((res) => {
          this.orders = res.data;
        })
        .catch((err) => {
          console.log(err);
        });
      this.$store.commit("setIsLoading", false);
    },
  },
};
</script>