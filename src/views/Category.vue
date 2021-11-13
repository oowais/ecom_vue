<template>
  <div class="page-category">
    <div class="columns is-multiline">
      <div class="column is-12">
        <h2 class="is-size-2 has-text-centered">{{ category.name }}</h2>
      </div>
      <div
        class="column is-3"
        v-for="product in category.products"
        :key="product.id"
      >
        <div class="box card">
          <figure class="image mb-4">
            <img :src="product.get_thumbnail" />
          </figure>

          <div class="card-details">
            <h3 class="is-size-5">{{ product.name }}</h3>
            <p class="is-size-6 has-text-grey">€{{ product.price }}</p>

            <router-link
              :to="product.get_absolute_url"
              class="button is-dark mt-4"
              >View Details</router-link
            >
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import axios from "axios";
import { toast } from "bulma-toast";
export default {
  name: "Category",
  data() {
    return {
      category: {
        products: [],
      },
    };
  },
  mounted() {
    this.getCategory();
  },
  methods: {
    async getCategory() {
      this.$store.commit("setIsLoading", true);
      const category_slug = this.$route.params.category_slug;

      await axios
        .get(`/api/v1/products/${category_slug}/`)
        .then((res) => {
          this.category = res.data;

          document.title = this.category.name + " | eCom";
        })
        .catch((err) => {
          console.log(err);

          toast({
            message: "Something went wrong, Please try again!",
            type: "is-danger",
            dismissible: true,
            pauseOnHover: true,
            duration: 2000,
            position: "bottom-center",
          });
        });

      this.$store.commit("setIsLoading", false);
    },
  },
};
</script>

<style scoped>
.image {
  width: 200px;
}

.card {
  width: 400px;
  height: 550px;
}

.card-details {
  position: absolute;
  bottom: 1em;
  margin-right: 0.5em;
}
</style>