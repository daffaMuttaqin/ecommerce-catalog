<script>
import axios from "axios";

export default {
  data() {
    return {
      product: null,
      currentIndex: 1,
      isLoading: false, //Menambahkan properti loading
    };
  },
  created() {
    this.fetchProduct(this.currentIndex);
  },
  methods: {
    async fetchProduct(index) {
      this.isLoading = true; // Set loading menjadi true sebelum memanggil API
      try {
        const response = await axios.get(
          `https://fakestoreapi.com/products/${index}`
        );
        this.product = response.data;
      } catch (error) {
        console.error("Error fetching product:", error);
      } finally {
        this.isLoading = false; // Set loading menjadi false setelah API dipanggil
      }
    },
    fetchNextProduct() {
      this.currentIndex = this.currentIndex === 20 ? 1 : this.currentIndex + 1;
      this.fetchProduct(this.currentIndex);
    },
  },
};
</script>

<template>
  <div id="app">
    <h1>Product Detail</h1>
    <!-- Menampilkan pesan loading -->
    <div v-if="isLoading">Loading...</div>
    <!-- End Loading -->
    <div v-else-if="product">
      <h2>{{ product.title }}</h2>
      <p>{{ product.description }}</p>
      <p>{{ product.category }}</p>
      <img :src="product.image" alt="" />
      <p>Price: ${{ product.price }}</p>
    </div>
    <button @click="fetchNextProduct">Next</button>
  </div>
</template>

<style scoped>
img {
  width: 50px;
}
</style>
