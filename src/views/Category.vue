<template>
  <div class="page-category">
    <div class="columns is-multiline">
      <div class="column is-12">
        <h2 class="is-size-2 has-text-centered">{{ category.name }}</h2>
      </div>
      <ProductCard
        v-for="product in category.products"
        :key="product.id"
        :product="product"
      ></ProductCard>
    </div>
  </div>
</template>

<script>
import axios from "axios";
import { toast } from "bulma-toast";
import ProductCard from "@/components/ProductCard";

export default {
  name: "Category",
  data() {
    return {
      category: {
        products: [],
      },
    };
  },
  components: {
    ProductCard,
  },
  mounted() {
    this.getCategory();
  },
  watch: {
    $route(to, from) {
      if (to.name === "Category") {
        this.getCategory();
      }
    },
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
