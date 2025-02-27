<script>
import axios from "axios";
import ProductDisplay from "./components/ProductDisplay.vue";

export default {
  components: {
    ProductDisplay,
  },
  data() {
    return {
      product: null,
      currentIndex: 1, //index di set default menjadi 1
      isLoading: false, //Menambahkan properti loading
    };
  },
  created() {
    this.fetchProduct(this.currentIndex);
  },
  methods: {
    // Fungsi untuk memanggil API menggunakan axios
    async fetchProduct(index) {
      this.isLoading = true; // Set loading menjadi true sebelum memanggil API
      try {
        const response = await axios.get(
          `https://fakestoreapi.com/products/${index}`
        );
        const productData = response.data;
        // Mengecek kategori men atau women
        if (
          productData.category === "men's clothing" ||
          productData.category === "women's clothing"
        ) {
          // Memasukkan data jika kategori men atau women
          this.product = productData;
        } else {
          // selain dari men atau women data tidak dimasukkan
          this.product = null;
        }
      } catch (error) {
        console.error("Error fetching product:", error);
      } finally {
        this.isLoading = false;
      }
    },
    // Membuat Fungsi untuk buttom next produk sampai dengan batas 20 produk
    fetchNextProduct() {
      this.currentIndex = this.currentIndex === 20 ? 1 : this.currentIndex + 1;
      this.fetchProduct(this.currentIndex);
    },
  },
};
</script>

<template>
  <div id="app">
    <ProductDisplay
      :product="product"
      :isLoading="isLoading"
      @fetch-next-product="fetchNextProduct"
    />
  </div>
</template>

<style scoped>
#app {
  height: 100%;
  width: 100%;
  position: relative;
  font-family: "Inter";
}
</style>
