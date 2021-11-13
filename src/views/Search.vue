<template>
  <div class="page-search">
    <div class="columns is-multiline">
      <div class="column is-12">
        <h1 class="title">Search Results</h1>
        <h2 class="is-size-5 has-text-grey">Search term: {{ query }}</h2>
      </div>
      <ProductCard
        v-for="product in products"
        :key="product.id"
        :product="product"
      ></ProductCard>
      <div v-if="products.length === 0">
        <h1 class="subtitle">No products related to {{ query }} 😒</h1>
      </div>
    </div>
  </div>
</template>

<script>
import axios from "axios";
import ProductCard from "@/components/ProductCard";

export default {
  name: "Search",
  components: {
    ProductCard,
  },
  data() {
    return {
      products: [],
      query: "",
    };
  },
  mounted() {
    document.title = "Search | eCom";

    let uri = window.location.search.substring(1);
    let params = new URLSearchParams(uri);

    if (params.get("query")) {
      this.query = params.get("query");

      this.performSearch();
    }
  },
  methods: {
    async performSearch() {
      this.$store.commit("setIsLoading", true);

      await axios
        .post("/api/v1/products/search/", { query: this.query })
        .then((res) => {
          this.products = res.data;
        })
        .catch((err) => {
          console.log(err);
        });

      this.$store.commit("setIsLoading", false);
    },
  },
};
</script>