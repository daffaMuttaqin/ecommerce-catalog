<script>
export default {
  props: {
    product: {
      type: Object,
      default: null,
    },
    isLoading: {
      type: Boolean,
      default: false,
    },
  },
  methods: {
    fetchNextProduct() {
      this.$emit("fetch-next-product");
    },
    // Fungsi untuk menentukan category
    getClassByCategory(category) {
      if (category === "men's clothing") {
        return "men-section";
      } else if (category === "women's clothing") {
        return "women-section";
      } else {
        return "unavailable-section";
      }
    },
    // Fungsi untuk rating
    getCircleClass(index) {
      const rate = this.product.rating.rate;
      const full = Math.floor(rate);
      const half = rate % 1 >= 0.5 ? full + 1 : full;

      if (index <= full) return "full";
      if (index === half) return "half";
      return "empty";
    },
  },
};
</script>

<template>
  <div :class="getClassByCategory(product?.category)">
    <!-- Loading Spinner -->
    <div class="bg-loader" v-if="isLoading">
      <span class="loader"></span>
    </div>
    <!-- End Loading -->

    <!-- Product Details -->
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

          <!-- Buttons -->
          <div class="button">
            <button class="btn-buy">Buy now</button>
            <button class="btn-next" @click="fetchNextProduct">
              Next product
            </button>
          </div>
        </div>
      </div>
    </div>

    <!-- Unavailable Product -->
    <div class="catalog" v-else>
      <div class="content">
        <div class="warning">
          <h1>This product is unavailable to show</h1>
          <button class="btn-next" @click="fetchNextProduct">
            Next Product
          </button>
        </div>
      </div>
    </div>
  </div>
</template>

<style scoped>
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
  background-image: url(../assets/bg-pattern.svg);
}
.men-section .catalog .content {
  height: 75%;
  width: 75%;
  background-color: white;
  border-radius: 10px;
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
      transition: background-color 0.3s ease, transform 0.3s ease,
        box-shadow 0.3s ease;
    }
    .btn-buy:hover {
      background-color: #001a4d;
      transform: translateY(-2px);
      box-shadow: 0 4px 6px rgba(0, 0, 0, 0.2);
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
      transition: background-color 0.3s ease, transform 0.3s ease,
        box-shadow 0.3s ease;
    }

    .btn-next:hover {
      background-color: white; /* Warna biru muda */
      transform: translateY(-2px); /* Bergerak sedikit ke atas */
      box-shadow: 0 4px 6px rgba(0, 0, 0, 0.2); /* Bayangan lebih terang */
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
  background-image: url(../assets/bg-pattern.svg);
}
.women-section .catalog .content {
  height: 75%;
  width: 75%;
  background-color: white;
  border-radius: 10px;
  box-shadow: 0 20px 25px -5px rgb(0 0 0 / 0.1),
    0 8px 10px -6px rgb(0 0 0 / 0.1);
  position: absolute;
  top: 50%;
  left: 50%;
  transform: translate(-50%, -50%);
  display: flex;
}
.women-section .catalog .content .product-image {
  width: 40%;
  height: 100%;
  display: flex;
  justify-content: center;
  align-items: center;
}
.women-section .catalog .content .detail-product {
  width: 60%;
  height: 100%;
  padding-top: 40px;
  padding-bottom: 40px;
  padding-right: 80px;

  .title {
    color: #720060;
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
      border: 2px solid #720060;
    }
    .full {
      background-color: #720060;
    }
    .half {
      background: linear-gradient(to right, #720060 50%, transparent 50%);
    }
    .empty {
      background-color: transparent;
    }
  }
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
    color: #720060;
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
      background-color: #720060;
      color: white;
      font-size: 20px;
      font-weight: 600;
      line-height: 24.2px;
      transition: background-color 0.3s ease, transform 0.3s ease,
        box-shadow 0.3s ease;
    }
    .btn-buy:hover {
      background-color: #46023c; /* Warna latar belakang saat hover */
      transform: scale(1.05); /* Sedikit memperbesar tombol */
      box-shadow: 0 4px 20px rgba(0, 0, 0, 0.2); /* Menambahkan bayangan */
    }

    .btn-next {
      width: 270px;
      height: 42px;
      border-radius: 4px;
      border-color: #720060;
      border-width: 3px;
      background-color: white;
      color: #720060;
      font-size: 20px;
      font-weight: 600;
      line-height: 24.2px;
      transition: background-color 0.3s ease, transform 0.3s ease,
        box-shadow 0.3s ease;
    }
    .btn-next:hover {
      background-color: white; /* Warna latar belakang saat hover */
      transform: scale(1.05); /* Sedikit memperbesar tombol */
      box-shadow: 0 4px 20px rgba(0, 0, 0, 0.2); /* Menambahkan bayangan */
    }
  }
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
  border-radius: 10px;
  background-color: white;
  background-image: url(../assets/sad-face-2.png);
  background-repeat: no-repeat;
  background-position: center;
  box-shadow: 0 20px 25px -5px rgb(0 0 0 / 0.1),
    0 8px 10px -6px rgb(0 0 0 / 0.1);
  position: absolute;
  top: 50%;
  left: 50%;
  transform: translate(-50%, -50%);

  .warning {
    width: 100%;
    height: 100%;
    display: flex;
    flex-direction: column;
    justify-content: center;
    align-items: center;
    h1 {
      font-size: 20px;
      font-weight: 400;
      line-height: 24.2px;
    }
    .btn-next {
      width: 465px;
      height: 42px;
      margin-top: 8px;
      border-radius: 4px;
      border-width: 3px;
      background-color: white;
      font-size: 20px;
      font-weight: 600;
      line-height: 24.2px;
      color: #3f3f3f;
      transition: background-color 0.3s ease, transform 0.3s ease,
        box-shadow 0.3s ease;
    }
    .btn-next:hover {
      background-color: white;
      transform: translateY(-2px);
      box-shadow: 0 4px 6px rgba(0, 0, 0, 0.2);
    }
  }
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
