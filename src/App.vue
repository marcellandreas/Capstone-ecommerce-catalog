<template>
  <div class="container">
    <div :class="sectionClass"></div>
    <div class="null"></div>
    <div class="card-product">
      <div
        style="display: flex; align-items: center; height: 100%"
        v-if="isLoading"
      >
        <div class="loader"></div>
      </div>
      <div v-else>
        <div v-if="products.length > 0">
          <div
            style="
              display: flex;
              height: 580px;
              justify-content: space-around;
              align-items: center;
            "
            v-for="product in products"
            :key="product.id"
          >
            <div>
              <img :src="product.image" :alt="product.title" />
            </div>

            <div
              style="
                height: 100%;
                width: 548px;
                display: flex;
                flex-direction: column;
                justify-content: space-around;
              "
            >
              <div>
                <h3 class="title-product" :class="colorClass">
                  {{ product.title }}
                </h3>
                <div class="second-product">
                  <p>
                    {{ product.category }}
                  </p>
                  <div style="display: flex; gap: 5px">
                    <p>{{ product.rating.rate }}</p>
                    <div style="display: flex; gap: 2px">
                      <p class="circle" :class="backgroundClass"></p>
                      <p class="circle" :class="backgroundClass"></p>
                      <p class="circle" :class="backgroundClass"></p>
                      <p class="circle" :class="borderClass"></p>
                      <p class="circle" :class="borderClass"></p>
                    </div>
                  </div>
                </div>
                <hr style="margin-bottom: 21px" />
                <p class="description-product">{{ product.description }}</p>
              </div>
              <div>
                <hr style="margin-bottom: 16.5px" />
                <p class="price-product" :class="colorClass">
                  ${{ product.price }}
                </p>
                <div style="display: flex; justify-content: space-around">
                  <button :class="backgroundClass">Buy Now</button>
                  <button
                    @click="getNextProduct"
                    :class="BorderButtonClass"
                    :disabled="isLoading"
                  >
                    Next Product
                  </button>
                </div>
              </div>
            </div>
          </div>
        </div>
        <div class="unavailable" v-else>
          <img class="unavailable-img" :src="image" alt="" />
          <div class="unvailable-content">
            <p class="unavailable-text">This product is unavailable to show</p>
            <button
              @click="getNextProduct"
              :class="BorderButtonClass"
              :disabled="isLoading"
            >
              Next Product
            </button>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  data() {
    return {
      products: [],
      index: 1,
      category: "",
      isLoading: false,
      image: require("@/assets/sad-face.png"),
    };
  },
  computed: {
    sectionClass() {
      if (this.category === "men's clothing") {
        return "men-section";
      } else if (this.category === "women's clothing") {
        return "women-section";
      } else {
        return "unavailable-product-section";
      }
    },
    BorderButtonClass() {
      if (this.category === "men's clothing") {
        return "men-border-button";
      } else if (this.category === "women's clothing") {
        return "women-border-button";
      } else {
        return "unavailable-border-button";
      }
    },
    colorClass() {
      if (this.category === "men's clothing") {
        return "color-mens";
      } else if (this.category === "women's clothing") {
        return "color-womens";
      } else {
        return "";
      }
    },
    backgroundClass() {
      if (this.category === "men's clothing") {
        return "bg-mens";
      } else if (this.category === "women's clothing") {
        return "bg-womens";
      } else {
        return "";
      }
    },
    borderClass() {
      if (this.category === "men's clothing") {
        return "border-mens";
      } else if (this.category === "women's clothing") {
        return "border-womens";
      } else {
        return "";
      }
    },
  },
  methods: {
    fetchProducts() {
      this.isLoading = true;
      fetch(`https://fakestoreapi.com/products/${this.index}`)
        .then((response) => response.json())
        .then((data) => {
          if (
            data.category === "men's clothing" ||
            data.category === "women's clothing"
          ) {
            this.products = [data];
            this.category = data.category;
          } else {
            this.products = [];
            this.category = "";
          }
        })
        .catch((error) => console.log(error))
        .finally(() => {
          this.isLoading = false;
        });
    },
    getNextProduct() {
      this.index++;
      if (this.index > 20) {
        this.index = 1;
      }
      this.fetchProducts();
    },
  },
  mounted() {
    this.fetchProducts();
  },
};
</script>

<style>
/* loader */
.loader {
  border: 4px solid #f3f3f3;
  border-top: 4px solid #3498db;
  border-radius: 50%;
  width: 30px;
  height: 30px;
  animation: spin 1s linear infinite;
  margin: 0 auto;
}

@keyframes spin {
  0% {
    transform: rotate(0deg);
  }
  100% {
    transform: rotate(360deg);
  }
}

/* Background section */
.men-section {
  /* CSS styles for Men's Clothing section */
  background-color: var(--men-section-color);
  height: 50%;
  width: 100%;
  position: relative;
  display: flex;
  justify-content: center;
  align-items: center;
}

.women-section {
  /* CSS styles for Women's Clothing section */
  background-color: var(--women-section-color);
  position: relative;
  height: 50%;
  width: 100%;
}

.unavailable-product-section {
  /* CSS styles for Unavailable Product section */
  background-color: var(--unavailable-section-color);
  position: relative;
  height: 50%;
  width: 100%;
}

/* Button  */

.men-border-button {
  color: var(--men-color);
  border: 2px solid var(--men-color);
  background-color: var(--white);
}
.women-border-button {
  color: var(--women-color);
  border: 2px solid var(--women-color);
  background-color: var(--white);
}
.unavailable-border-button {
  border-radius: 4px;
  border: 3px solid #000;
  background: #fff;
  color: var(--text-color);
}

/* Color Text */
.color-mens {
  color: var(--men-color);
}
.color-womens {
  color: var(--women-color);
}

/* Circle */
.bg-mens {
  background: var(--men-color);
}
.bg-womens {
  background: var(--women-color);
}

.border-mens {
  border: 2px solid var(--men-color);
}
.border-womens {
  border: 2px solid var(--women-color);
}
</style>
