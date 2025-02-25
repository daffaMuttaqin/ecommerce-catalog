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
    getCircleClass(index) {
      const full = Math.floor(this.product.rating.rate);
      const half = this.product.rating.rate % 1 >= 0.5 ? full + 1 : full;
      if (index <= full) return "full";
      if (index === half) return "half";
      return "empty";
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
            <h1 class="title">{{ product.title }}</h1>
            <div class="category">
              <p>{{ product.category }}</p>
              <div class="rating">
                <span>{{ product.rating.rate }}/5</span>
                <div class="circles">
                  <div v-for="n in 5" :key="n" :class="getCircleClass(n)"></div>
                </div>
              </div>
            </div>
            <hr />

            <p class="description">{{ product.description }}</p>

            <hr />

            <!-- Price -->
            <div class="price">${{ product.price }}</div>

            <!-- Button -->
            <div class="button">
              <button class="btn-buy">Buy now</button>
              <button class="btn-next" @click="fetchNextProduct">
                Next product
              </button>
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
  font-family: "Inter";
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
  width: 40%;
  height: 100%;
  display: flex;
  justify-content: center;
  align-items: center;
}

.men-section .catalog .content .detail-product {
  width: 60%;
  height: 100%;
  padding-top: 40px;
  padding-bottom: 40px;
  padding-right: 80px;

  .title {
    color: #002772;
  }
  .category {
    display: flex;
    color: #3f3f3f;
    justify-content: space-between;
    padding-top: 20px;
    padding-bottom: 10px;
    font-size: 18px;
    font-weight: 400;
    line-height: 21.78px;

    .rating {
      display: flex;
      align-items: center;
    }

    .circles {
      display: flex;
      margin-left: 8px;
    }

    .circles div {
      width: 16px;
      height: 16px;
      border-radius: 50%;
      margin-right: 4px;
      border: 2px solid #002772;
    }

    .full {
      background-color: #002772;
    }

    .half {
      background: linear-gradient(to right, #002772 50%, transparent 50%);
    }

    .empty {
      background-color: transparent;
    }
  }

  /* .rating {
    display: flex;
    p {
      padding-right: 5px;
    }
    .fill {
      width: 18px;
      height: 18px;
      margin-left: 1px;
      border-radius: 9999px;
      background-color: #002772;
    }
    .none {
      width: 18px;
      height: 18px;
      margin-left: 1px;
      border-radius: 9999px;
      border-color: #002772;
      border-width: 2px;
      background-color: white;
    }
  } */
  .description {
    color: #1e1e1e;
    padding-top: 20px;
    height: 50%;
    font-size: 20px;
    font-weight: 400;
    line-height: 24.2px;
  }
  .price {
    padding-top: 20px;
    padding-bottom: 20px;
    color: #002772;
    font-size: 28px;
    font-weight: 600;
    line-height: 33.89px;
  }
  .button {
    display: flex;
    justify-content: space-between;

    .btn-buy {
      width: 270px;
      height: 42px;
      border-radius: 4px;
      background-color: #002772;
      color: white;
      font-size: 20px;
      font-weight: 600;
      line-height: 24.2px;
    }

    .btn-next {
      width: 270px;
      height: 42px;
      border-radius: 4px;
      border-color: #002772;
      border-width: 3px;
      background-color: white;
      color: #002772;
      font-size: 20px;
      font-weight: 600;
      line-height: 24.2px;
    }
  }
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
