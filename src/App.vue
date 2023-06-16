<template>
  <div id="app" class="kotak">
    <div class="container" :class="getSectionClass()">
      <div class="images2" v-if="!loading">
      <img class="gambar1" style="width: 500px;" src="./assets/halo.jpg" alt="" v-if="!loading"/>
      <h1>This Product Unvailable now</h1>
      </div>
      <div class="images">
        <img :src="product.image" alt="Product Image" v-if="!loading" />
        <div class="loading-animation" v-else>
          Loading...
        </div>
      </div>
      <div class="product" v-if="!loading">
        <h1>{{ product.title }}</h1>
        <div class="rating">
          <p>{{ product.rating }}/5</p>
          <p>{{ product.category }}</p>
        </div>
        <hr />
        <p class="desc">{{ product.description }}</p>
        <div class="buttons">
          <h2 class="harga">{{ product.price }}</h2>
          <button class="add">Buy Now</button>
          <button class="like" @click="getNextProduct">Next Product</button>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import axios from 'axios';

export default {
  data() {
    return {
      currentIndex: 1,
      product: {},
      loading: false
    };
  },
  mounted() {
    this.fetchProduct();
  },
  methods: {
    async fetchProduct() {
      try {
        this.loading = true; // Display loading animation
        const response = await axios.get(
          `https://fakestoreapi.com/products/${this.currentIndex}`
        );
        this.product = response.data;
      } catch (error) {
        console.error('Error fetching product:', error);
      } finally {
        this.loading = false; // Hide loading animation after completion
      }
    },
    getNextProduct() {
      if (this.currentIndex < 20) {
        this.currentIndex++;
      } else {
        this.currentIndex = 1;
      }
      this.fetchProduct();
    },
    getSectionClass() {
  if (this.product.category === "men's clothing") {
    return 'page-men';
  } else if (this.product.category === "women's clothing") {
    return 'page-women';
  } else {
    return 'page-unavailable';
  }
}
  }
};
</script>

<style src="./assets/style/style.css">

</style>