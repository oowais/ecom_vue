<template>
  <div class="page-log-in">
    <div class="columns">
      <div class="column is-4 is-offset-4">
        <h1 class="title">Log In</h1>

        <form @submit.prevent="submitForm">
          <div class="field">
            <label>Username</label>
            <div class="control">
              <input type="text" class="input" v-model="username" />
            </div>
          </div>
          <div class="field">
            <label>Password</label>
            <div class="control">
              <input type="password" class="input" v-model="password" />
            </div>
          </div>

          <div class="notification is-danger" v-if="errors.length">
            <p v-for="error in errors" :key="error">{{ error }}</p>
          </div>

          <div class="field">
            <div class="control">
              <button class="button is-dark">Login</button>
            </div>
          </div>

          <hr />
          Or <router-link to="/sign-up">Sign up</router-link>
        </form>
      </div>
    </div>
  </div>
</template>

<script>
import axios from "axios";

export default {
  name: "LogIn",
  data() {
    return {
      username: "",
      password: "",
      errors: [],
    };
  },
  mounted() {
    document.title = "Login | eCom";
  },
  methods: {
    async submitForm() {
      localStorage.removeItem("token");

      const formData = {
        username: this.username,
        password: this.password,
      };

      axios
        .post("/api/v1/token/login", formData)
        .then((res) => {
          const token = res.data.auth_token;
          this.$store.commit("setToken", token);

          axios.defaults.headers.common["Authoization"] = "Token " + token;
          localStorage.setItem("token", token);

          const toPath = this.$route.query.to || "/cart";
          this.$router.push(toPath);
        })
        .catch((err) => {
          if (err.response) {
            for (const property in err.response.data) {
              this.errors.push(`${property}: ${err.response.data[property]}`);
            }

            console.log(JSON.stringify(err.res.data));
          } else if (err.message) {
            this.errors.push("Something went wrong, please try again.");

            console.log(JSON.stringify(err));
          }
        });
    },
  },
};
</script>