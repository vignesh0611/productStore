<template>
  <div class="page--width">
    <div class="home">
      <a class="active float__start" href="#home">Home</a>
      <a href="#news" class="float__end">Login</a>
      <a href="#contact " class="float__end">Contact</a>
      <a href="#about" class="float__end">About</a>
      <a href="#cart" class="float__end">
        <img src="../assets/cart.svg" alt="cart" height="14px" /> Cart
        <span class="number--badge"> {{ favoriteProduct.length }}</span>
      </a>
    </div>
    <div class="grid-container">
      <div
        class="grid-item"
        v-for="(product, index) in fetchedProduct"
        :value="product"
        :key="index"
        @click="favoriteIcon(product)"
        @mouseover="showPrice(product)"
        @mouseleave="hideprice(product)"
      >
        <div class="boxStyle">
          <span v-for="(fav, index) in displayIcon" :value="fav" :key="index">
            <img
              src="../assets/heart-svgrepo-com.svg"
              class="favorite--icon"
              v-if="fav.product == product.product"
              alt="Favorite"
              height="30vh"
              width="30vh"
            />
          </span>
          <div>{{ product.product }}<br /></div>
          <span
            class="price--text"
            v-if="priceToDisplay && productToDisplayPrice == product.product"
          >
            Price: {{ product.price }}
          </span>
        </div>
      </div>
    </div>
    <div id="cart" class="cart--align" v-if="cartData.length > 0">
      <h2>Products in cart</h2>
      <table class="customers">
        <tr>
          <th>Product</th>
          <th>Quantity</th>
        </tr>
        <tr v-for="(product, index) in cartData" :value="product" :key="index">
          <td>{{ product.key }}</td>
          <td>{{ product.value }}</td>
        </tr>
      </table>
    </div>
  </div>
</template>
  
  <script>
import jsonProduct from "../data.json";
export default {
  name: "Home",
  data() {
    return {
      fetchedProduct: [],
      priceToDisplay: false,
      productToDisplayPrice: null,
      displayIcon: null,
      favoriteIndex: null,
      favoriteProduct: jsonProduct.product,
      cartData: [],
    };
  },
  mounted() {
    this.fetchData();
  },
  methods: {
    fetchData() {
      fetch("https://neodigm.github.io/FED_Programming_Challenge/products.json")
        .then((response) => {
          response.json().then((data) => {
            this.fetchedProduct = data;
            console.log(data);
          });
        })
        .catch((err) => {
          console.error(err);
        });
    },
    showPrice(product) {
      this.priceToDisplay = true;
      this.productToDisplayPrice = product.product;
    },
    hideprice() {
      this.priceToDisplay = false;
      this.productToDisplayPrice = null;
    },
    favoriteIcon(product) {
      let favoriteProduct = product;
      this.favoriteProduct.push(favoriteProduct);
      let favCount1 = this.favoriteProduct.map((a) => a.product);
      this.displayIcon = this.favoriteProduct.filter(
        (value, index, self) =>
          index === self.findIndex((t) => t.product === value.product)
      );
      let favCount2 = this.displayIcon.map((a) => a.product);
      this.cartData = [];
      for (let i = 0; i < favCount2.length; i++) {
        let count = 0;
        for (let j = 0; j < favCount1.length; j++) {
          if (favCount2[i] == favCount1[j]) {
            count++;
          }
        }
        this.cartData.push({ key: favCount2[i], value: count });
      }
    },
  },
};
</script>
<style scoped>
.page--width {
  width: fit-content;
}

body {
  margin: 0;
  font-family: Arial, Helvetica, sans-serif;
}

.home {
  overflow: hidden;
  background-color: #3d6bfc;
}

.home a {
  color: #f2f2f2;
  text-align: center;
  padding: 14px 16px;
  text-decoration: none;
  font-size: 22;
}

.home a:hover {
  background-color: #ddd;
  color: black;
}

.home a.active {
  background-color: #0a2885;
  color: white;
}
.float__start {
  float: inline-start;
}
.float__end {
  float: inline-end;
}

.grid-container {
  display: grid;
  grid-template-columns: auto auto auto;
  background-color: #fff;
  padding-top: 10px;
}
.grid-item {
  background-color: #e7f1ff;
  padding: 20px;
  font-size: 30px;
  text-align: center;
}

.boxStyle {
  border-radius: 12px;
  position: relative;
  height: 35vh;
  width: 62vh;
  border: 3px solid #0f3cc9;
}
.boxStyle div {
  margin: 0;
  position: absolute;
  top: 50%;
  left: 50%;
  -ms-transform: translate(-50%, -50%);
  transform: translate(-50%, -50%);
}

.favorite--icon {
  float: inline-end;
  padding: 1vh;
}
.number--badge {
  font-size: 14px;
  font-weight: 900;
  position: absolute;
  border: 3px solid #0f3cc9;
  border-radius: 60%;
  height: 14px;
  width: 8px;
  background: blue;
}

.price--text {
  position: absolute;
  top: 85%;
  left: 50%;
  transform: translate(-50%, -50%);
  font-size: 20px;
  border: 3px solid #ff7a05;
  border-radius: 5px;
  padding: 5px;
}
.cart--align {
  text-align: center;
  width: 50%;
  margin-left: auto;
  margin-right: auto;
}

.customers {
  font-family: Arial, Helvetica, sans-serif;
  border-collapse: collapse;
  width: 100%;
}

.customers td,
#customers th {
  border: 1px solid #ddd;
  padding: 8px;
}

.customers tr:nth-child(even) {
  background-color: #f2f2f2;
}

.customers tr:hover {
  background-color: #ddd;
}

.customers th {
  padding-top: 12px;
  padding-bottom: 12px;
  background-color: #3d6bfc;
  color: white;
}
</style>
  