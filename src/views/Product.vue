<template>
  <div class="page-product">
    <div class="columns is-multiline">
      <div class="column is-5">
        <figure class="image mb-6">
          <img :src="product.get_image" />
        </figure>
      </div>
      <!-- <div class="2">
          <span></span>
      </div> -->

      <div class="column is-7">
        <h1 class="title">{{ product.name }}</h1>
        <br />
        <p class="subtitle"><strong>Price: </strong>€{{ product.price }}</p>
        <h2 class="subtitle">{{ product.description }}</h2>

        <div class="field has-addons mt-6">
          <div class="control">
            <input type="number" class="input" min="1" v-model="quantity" />
          </div>

          <div class="contro">
            <a class="button is-dark">Add to Cart</a>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import axios from "axios";

export default {
  name: "Product",
  data() {
    return {
      product: {},
      quantity: 1,
    };
  },
  mounted() {
    this.getProduct();
  },
  methods: {
    getProduct() {
      const category_slug = this.$route.params.category_slug;
      const product_slug = this.$route.params.product_slug;
      axios
        .get(`/api/v1/products/${category_slug}/${product_slug}/`)
        .then((res) => {
          this.product = res.data;
        })
        .catch((err) => {
          console.log(err);
        });
    },
  },
};
</script>
