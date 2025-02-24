<script>
import axios from "axios";

export default {
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
    // Memanggil API menggunakan Axios
    async fetchProduct(index) {
      this.isLoading = true; // Set loading menjadi true sebelum memanggil API
      try {
        const response = await axios.get(
          `https://fakestoreapi.com/products/${index}`
        );
        const productData = response.data;
        // Memeriksa kategori produk
        if (
          productData.category === "men's clothing" ||
          productData.category === "women's clothing"
        ) {
          this.product = productData;
        } else {
          this.product = null;
        }

        console.log(this.product);
      } catch (error) {
        console.error("Error fetching product:", error);
      } finally {
        this.isLoading = false; // Set loading menjadi false setelah API dipanggil
      }
    },
    // Membuat fungsi untuk tombol next
    fetchNextProduct() {
      this.currentIndex = this.currentIndex === 20 ? 1 : this.currentIndex + 1;
      this.fetchProduct(this.currentIndex);
    },
    // Membuat fungsi untuk mengambil nilai kategori pria atau wanita
    getClassByCategory(category) {
      if (category === "men's clothing") {
        return "men-section";
      } else if (category === "women's clothing") {
        return "women-section";
      } else {
        return "unavailable-section";
      }
    },
  },
};
</script>

<template>
  <div id="app">
    <div :class="getClassByCategory(product?.category)">
      <!-- Menampilkan spinner loading -->
      <div class="bg-loader" v-if="isLoading">
        <span class="loader"></span>
      </div>
      <!-- End Loading -->

      <!-- Mengecek jika produk nya ada -->
      <div class="catalog" v-else-if="product">
        <div class="content">
          <div class="product-image">
            <img class="image" :src="product.image" alt="" />
          </div>
          <div class="detail-product">
            <h1>{{ product.title }}</h1>
            <div class="category">
              <p>{{ product.category }}</p>
              <p>{{ product.rating.rate }}/5</p>
            </div>
            <hr />

            <p>{{ product.description }}</p>

            <hr />

            <!-- Price -->
            <div>{{ product.price }}</div>

            <!-- Button -->
            <div>
              <button>Buy now</button>
              <button @click="fetchNextProduct">Next product</button>
            </div>
          </div>
        </div>
      </div>

      <div class="catalog" v-else>
        <div class="content">
          <button @click="fetchNextProduct">Next</button>
        </div>
      </div>
    </div>
  </div>
</template>

<style scoped>
#app {
  height: 100%;
  width: 100%;
  position: relative;
}

.image {
  width: 301px;
  height: 383px;
}

/* MEN SECTION */
.men-section {
  background-color: #d6e6ff;
  height: 75%;
}
.men-section .catalog {
  height: 100%;
  background-image: url(./assets/bg-pattern.svg);
}
.men-section .catalog .content {
  height: 75%;
  width: 75%;
  background-color: white;
  box-shadow: 0 20px 25px -5px rgb(0 0 0 / 0.1),
    0 8px 10px -6px rgb(0 0 0 / 0.1);
  position: absolute;
  top: 50%;
  left: 50%;
  transform: translate(-50%, -50%);
  display: flex;
}

.men-section .catalog .content .product-image {
  width: 30%;
  height: 100%;
  display: flex;
  justify-content: center;
  align-items: center;
}

.men-section .catalog .content .detail-product {
  h1 {
    color: #002772;
  }
  .category {
    display: flex;
  }
  width: 70%;
  height: 100%;
  padding-top: 40px;
  padding-bottom: 40px;
  padding-right: 40px;
}

/* END MEN SECTION */

/* WOMEN SECTION */
.women-section {
  background-color: #fde2ff;
  height: 75%;
}
.women-section .catalog {
  height: 100%;
  background-image: url(./assets/bg-pattern.svg);
}
.women-section .catalog .content {
  height: 75%;
  width: 75%;
  background-color: white;
  box-shadow: 0 20px 25px -5px rgb(0 0 0 / 0.1),
    0 8px 10px -6px rgb(0 0 0 / 0.1);
  position: absolute;
  top: 50%;
  left: 50%;
  transform: translate(-50%, -50%);
}

/* END WOMEN SECTION */

/* UNAVAILABLE SECTION */
.unavailable-section {
  background-color: #dcdcdc;
  height: 75%;
}

.unavailable-section .catalog {
  height: 100%;
}
.unavailable-section .catalog .content {
  height: 75%;
  width: 75%;
  background-color: white;
  box-shadow: 0 20px 25px -5px rgb(0 0 0 / 0.1),
    0 8px 10px -6px rgb(0 0 0 / 0.1);
  position: absolute;
  top: 50%;
  left: 50%;
  transform: translate(-50%, -50%);
}

/* END UNAVAILABLE SECTION */

/* Style Loading */
.bg-loader {
  height: 100%;
  width: 100%;
  background-color: white;
}
.loader {
  width: 48px;
  height: 48px;
  border: 5px solid #002772;
  border-radius: 50%;
  display: inline-block;
  box-sizing: border-box;
  position: relative;
  animation: pulse 1s linear infinite;
  position: fixed;
  top: 50%;
  left: 50%;
}
.loader:after {
  content: "";
  position: absolute;
  width: 48px;
  height: 48px;
  border: 5px solid #002772;
  border-radius: 50%;
  display: inline-block;
  box-sizing: border-box;
  left: 50%;
  top: 50%;
  transform: translate(-50%, -50%);
  animation: scaleUp 1s linear infinite;
}

@keyframes scaleUp {
  0% {
    transform: translate(-50%, -50%) scale(0);
  }
  60%,
  100% {
    transform: translate(-50%, -50%) scale(1);
  }
}
@keyframes pulse {
  0%,
  60%,
  100% {
    transform: scale(1);
  }
  80% {
    transform: scale(1.2);
  }
}
/* End Style Loading */
</style>
