<template>
  <div class="page-checkout">
    <div class="columns is-multiline">
      <div class="column is-12">
        <h1 class="title">Checkout</h1>
      </div>

      <div class="column is-12 box">
        <table class="table is-fullwidth">
          <thead>
            <tr>
              <th>Product</th>
              <th>Price</th>
              <th>Quantity</th>
              <th>Total</th>
            </tr>
          </thead>
          <tbody>
            <tr v-for="item in cart.items" :key="item.product.id">
              <td>{{ item.product.name }}</td>
              <td>€{{ item.product.price }}</td>
              <td>{{ item.quantity }}</td>
              <td>€{{ getItemTotal(item).toFixed(2) }}</td>
            </tr>
          </tbody>

          <tfoot>
            <tr>
              <td colspan="2">Total</td>
              <td>{{ cartTotalLength }}</td>
              <td>€{{ cartTotalPrice.toFixed(2) }}</td>
            </tr>
          </tfoot>
        </table>
      </div>

      <div class="column is-12 box">
        <h2 class="subtitle">Shipping details</h2>

        <p class="has-text-grey mb-4">*All fields are required</p>

        <div class="columns is-multiline">
          <div class="column is-6">
            <div class="field">
              <label>First name*</label>
              <div class="control">
                <input type="text" class="input" v-model="first_name" />
              </div>
            </div>

            <div class="field">
              <label>Last name*</label>
              <div class="control">
                <input type="text" class="input" v-model="last_name" />
              </div>
            </div>

            <div class="field">
              <label>Email*</label>
              <div class="control">
                <input type="email" class="input" v-model="email" />
              </div>
            </div>

            <div class="field">
              <label>Phone*</label>
              <div class="control">
                <input type="text" class="input" v-model="phone" />
              </div>
            </div>
          </div>

          <div class="column is-6">
            <div class="field">
              <label>Address*</label>
              <div class="control">
                <input type="text" class="input" v-model="address" />
              </div>
            </div>

            <div class="field">
              <label>Zip code*</label>
              <div class="control">
                <input type="text" class="input" v-model="zipcode" />
              </div>
            </div>

            <div class="field">
              <label>Place*</label>
              <div class="control">
                <input type="email" class="input" v-model="place" />
              </div>
            </div>
          </div>
        </div>
        <div class="notification is-danger mt-4" v-if="errors.length">
          <p v-for="error in errors" :key="error">{{ error }}</p>
        </div>

        <div id="card-element" class="mb-5"></div>

        <template v-if="cartTotalLength">
          <hr />
          <button class="button is-dark" @click="submitForm">Pay</button>
        </template>
        <template else></template>
      </div>
    </div>
  </div>
</template>

<script>
import axios from "axios";
import { toast } from "bulma-toast";
import LogInVue from "./LogIn.vue";

export default {
  name: "Checkout",
  data() {
    return {
      cart: {
        items: [],
      },
      strpe: {},
      card: {},
      first_name: "",
      last_name: "",
      email: "",
      phone: "",
      address: "",
      zipcode: "",
      place: "",
      errors: [],
    };
  },
  mounted() {
    document.title = "Checkout | eCom";

    this.cart = this.$store.state.cart;
  },
  methods: {
    getItemTotal(item) {
      return item.quantity * item.product.price;
    },
    submitForm() {
      this.errors = [];

      if (this.first_name === "") {
        this.errors.push("First name is missing!");
      }
      if (this.last_name === "") {
        this.errors.push("Last name is missing!");
      }
      if (this.email === "") {
        this.errors.push("Email Address is missing!");
      }
      if (this.phone === "") {
        this.errors.push("Phone is missing!");
      }
      if (this.address === "") {
        this.errors.push("Address is missing!");
      }
      if (this.zipcode === "") {
        this.errors.push("Zipcode is missing!");
      }
      if (this.place === "") {
        this.errors.push("Place is missing!");
      }
      //need to clear the cart and add order
      if (!this.errors.length) {
        this.$store.commit("setIsLoading", true);

        this.onSubmitHandler();
      }
    },
    async onSubmitHandler() {
      const items = [];
      for (let i = 0; i < this.cart.items.length; i++) {
        const item = this.cart.items[i];
        const obj = {
          product: item.product.id,
          quantity: item.quantity,
          price: item.quantity * item.product.price,
        };
        items.push(obj);
      }

      const data = {
        first_name: this.first_name,
        last_name: this.last_name,
        email: this.email,
        address: this.address,
        zipcode: this.zipcode,
        place: this.place,
        phone: this.phone,
        items: items,
      };

      await axios
        .post("/api/v1/checkout/", data)
        .then((res) => {
          this.$store.commit("clearCart");
          this.$router.push("/cart/success");
        })
        .catch((err) => {
          this.errors.push("Something went wrong, try again! 😢");
          console.log(err);
        });
      this.$store.commit("setIsLoading", false);
    },
  },
  computed: {
    cartTotalLength() {
      return this.cart.items.reduce((acc, curVal) => {
        return (acc += curVal.quantity);
      }, 0);
    },
    cartTotalPrice() {
      return this.cart.items.reduce((acc, curVal) => {
        return (acc += curVal.quantity * curVal.product.price);
      }, 0);
    },
  },
};
</script>