<template>
  <div class="column is-3">
    <div class="box card">
      <figure class="image mb-4">
        <img :src="product.get_thumbnail" />
      </figure>

      <div class="card-details">
        <h3 class="is-size-5">{{ product.name }}</h3>
        <p class="is-size-6 has-text-grey">€{{ product.price }}</p>

        <router-link :to="product.get_absolute_url" class="button is-dark mt-4"
          >View Details</router-link
        >
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: "ProductCard",
  props: {
    product: Object,
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