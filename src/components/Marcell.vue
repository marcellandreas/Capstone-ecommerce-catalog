<template>
  <div>
    <div class="cards">
      <!-- <div :class="sectionClass"></div>
       <div class="null"></div> -->
      <div class="card-product">
        <!-- <h2>{{ sectionTitle }}</h2> -->
        <div style="background: yellow; height: 100%; position: relative">
          <div style="position: absolute" v-if="isLoading">
            <div class="loader"></div>
          </div>
          <div style="height: 100%" v-else>
            <div style="height: 100%" v-if="products">
              <div
                style="
                  display: flex;
                  background-color: aqua;
                  height: 100%;
                  justify-content: space-around;
                  align-items: center;
                "
                v-for="product in products"
                :key="product.id"
              >
                <!-- Image -->
                <div class="image_product">
                  <img :src="products[0].image" />
                </div>
                <!-- Content -->
                <div
                  style="
                    height: 100%;
                    width: 540px;
                    display: flex;
                    flex-direction: column;
                    background-color: aquamarine;
                    justify-content: space-around;
                  "
                >
                  <div>
                    <h3 class="title-product">{{ product.title }}</h3>
                    <div
                      style="
                        display: flex;

                        justify-content: space-between;
                        padding: 0 10px;
                        margin-bottom: 11px;
                        color: #3f3f3f;
                        font-size: 18px;
                        font-family: Inter;
                        font-style: normal;
                        font-weight: 400;
                        line-height: normal;
                      "
                    >
                      <p>
                        {{ product.category }}
                      </p>
                      <p>{{ product.rating.rate }}</p>
                    </div>
                    <hr style="margin-bottom: 21px" />
                    <p class="description-product">{{ product.description }}</p>
                  </div>
                  <div>
                    <hr style="margin-bottom: 16.5px" />
                    <p class="price-product">${{ product.price }}</p>
                    <div style="display: flex">
                      <button>Buy Now</button>
                      <button @click="getNextProduct" :disabled="isLoading">
                        Next Product
                      </button>
                    </div>
                  </div>
                </div>
              </div>
            </div>
            <div v-else>
              <p>No products available.</p>
              <button @click="getNextProduct" :disabled="isLoading">
                Next Product
              </button>
            </div>
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
    };
  },
  computed: {
    sectionTitle() {
      if (this.category === "men's clothing") {
        return "Men's Clothing";
      } else if (this.category === "women's clothing") {
        return "Women's Clothing";
      } else {
        return "Product Unavailable";
      }
    },
    sectionClass() {
      if (this.category === "men's clothing") {
        return "men-section";
      } else if (this.category === "women's clothing") {
        return "women-section";
      } else {
        return "unavailable-product-section";
      }
    },
  },
  methods: {
    fetchProducts() {
      this.isLoading = true;
      fetch(`https://fakestoreapi.com/products/${this.index}`)
        .then((response) => response.json())
        .then((data) => {
          this.products = [data];
          this.category = data.category;
          console.log(data);
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
* {
  padding: 0;
  margin: 0;
  box-sizing: border-box;
}
:root {
  --men-color: #002772;
  --men-section-color: #d6e6ff;
  --women-section-color: #fde2ff;
  --unavailable-section-color: #dcdcdc;
  --white: #fff;
}
.cards {
  /* background-color: yellow; */
  width: 100%;
  height: 100vh;
}
.card-product {
  margin: auto;
  height: 580px;
  width: 80%;
  position: absolute;
  left: 50%;
  top: 50%;
  transform: translate(-50%, -50%);
  border-radius: 10px;
  background: #fff;
  box-shadow: 0px 4px 4px 0px rgba(0, 0, 0, 0.25);
}

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

.null {
  background: #fff;
  height: 50%;
  position: relative;
}

/* Component Product */
.title-product {
  color: #002772;
  font-size: 28px;
  font-family: Inter;
  font-style: normal;
  font-weight: 600;
  line-height: normal;
  margin-bottom: 16px;
}

.description-product {
  color: #1e1e1e;
  font-size: 20px;
  font-family: Inter;
  font-style: normal;
  font-weight: 400;
  line-height: normal;
}

.price-product {
  color: #002772;
  font-size: 28px;
  font-family: Inter;
  font-style: normal;
  font-weight: 600;
  line-height: normal;
}

img {
  width: 320px;
  height: 400px;
}

/* Styles for loader */
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

/* Rest of the styles */
/* ... */
</style>
