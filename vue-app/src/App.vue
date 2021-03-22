<template>
  <div id="app">
    <nav>    
      <div class="cart">
        <h3>Vue Store</h3>
        <p class="grey-box">Cart({{cart.length}})</p>
      </div>
    </nav>

    <div class="product">
      <div class="product-image">
        <!-- v-bind omitted -->
        <img class="grey-box" :src="image" />
      </div>

      <div class="product-info">
      <h1>{{ title }}</h1>
      <p v-if="inStock > 10">In Stock</p>
      <p v-else-if="inStock <= 10 && inStock > 0">Only {{inStock}} left!</p>
      <p v-else class="red">Out of Stock</p>
      <p v-if="onSale" class="price">On SALE for just £{{salePrice}}</p>
      <p v-else class="price">£{{price}}</p>

      <p class="shipping">{{ shipping }} Shipping</p>

      <ul>
        <li v-for="detail in details" :key="detail.id">
          {{detail}}
        </li>
      </ul>

      <div class="size-select">
        <p>Please select a size:</p>
        <select>
          <option v-for="size in sizes" :key="size.id">
            {{size}}
          </option>
        </select>
      </div>

      <p>Please select a color:</p>
      <!-- v-bind and v-on omitted -->
      <div class="flex-wrapper">
        <div v-for="(variant, index) in variants" 
          :key="variant.variantId"
          class="color-box"
          :style="{ backgroundColor: variant.variantColor}"
          @mouseover="updateProduct(index)">
        </div>
      </div>

      <br />

      <!-- v-bind omitted -->
      <button v-on:click="addToCart"
        :class="{ disabledBtn: inStock < 1 }">Add 1 to Cart</button>
      <button v-on:click="removeFromCart"
        :class="{ disabledBtn: cart < 1 }">Remove {{ variants[selectedVariant].variantColor }} {{ product }} from Cart</button>

      </div>
      </div>

    
  </div>

</template>



<script>
//import Shoes from './components/Shoes.vue'
import redsox from './images/sox.png'
import bluesox from './images/bluesox.png'

export default {
  name: 'App',
  data() {
    return {
      premium: true,
      cart: [],
      product: 'Socks',
      brand: 'Boston Sports',
      selectedVariant: 0,
      price: 19.99,
      onSale: true,
      salePrice: 9.99,
      details: ['80% cotton', '20% polyester', 'one-size-fits-all'],
      sizes: ['S', 'M', 'L'],
      variants: [
        {
          variantId: 2234,
          variantColor: 'Red',
          variantImage: redsox,
          variantQuantity: 7
        },
        {
          variantId: 2235,
          variantColor: 'Blue',
          variantImage: bluesox,
          variantQuantity: 11
        },
      ],
    }
  },
  methods: {
    addToCart() {
      if (this.variants[this.selectedVariant].variantQuantity > 0) {
        this.cart.push(this.variants[this.selectedVariant].variantId)
        this.variants[this.selectedVariant].variantQuantity -= 1
      }
    },
    removeFromCart() {
      this.cart = this.cart.filter(item => {
        return item !== this.variants[this.selectedVariant].variantId
      })
      //this.variants[this.selectedVariant].variantQuantity += 1
    },
    updateProduct(index) {
      this.selectedVariant = index
    }
  },
  computed: {
    title() {
      return this.brand + ' ' + this.product
    },
    image() {
      return this.variants[this.selectedVariant].variantImage
    },
    inStock() {
      return this.variants[this.selectedVariant].variantQuantity
    },
    shipping() {
      if (this.premium) {
        return "Free" 
      } 
      return `£${2.99}`
    }
  },
}



</script>



<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
  margin: 0;
}

body {
  font-family: Arial, Helvetica, sans-serif;
  margin: 0;
}

nav {
  background: linear-gradient(to right, teal, yellowgreen);
  width: 100%;
  height: 60px;
  margin: 0;
  position: fixed;
  top: 0;
  box-shadow: 0 0 5px 1px rgba(0, 0, 0, 0.3);
}

.product {
  display: flex;
  justify-content: space-evenly;
  padding: 20px;
  margin-top: 5rem;
}

img {
  width: 300px;
}

.grey-box {
  border: 1px solid rgb(196, 196, 196);
  text-align: center;
  margin: 5px;
  padding: 15px;
}

.color-box {
  width: 30px;
  height: 30px;
  margin-right: 5px;
  cursor: pointer;
}

.cart {
  display: flex;
  justify-content: space-between;
  align-items: center;
  margin: 0 2rem;
}

.cart p {
  background-color: white;
}

.cart h3 {
  color: white; 
  padding-right: 20px;
  font-size: 1.7rem;
  margin: 0;
  text-transform: uppercase;
}

ul {
  list-style: none;
}

button {
  cursor: pointer;
  background-color: rgb(14, 141, 238);
  border: none;
  border-radius: 10px;
  padding: 12px;
  color: white;
  margin: 5px;
}

button:hover {
  background-color: rgb(14, 107, 179);
}

select {
  width: 100px;
  padding: 5px;
  cursor: pointer;
}

.disabledBtn {
  background-color: rgb(196, 196, 196);
  cursor: default;
}

.chart {
  width: 90%;
  margin: 2rem auto;
  height: 500px;
  display: flex;
  justify-content: center;
  align-items: center;
  border: 1px solid rgb(196, 196, 196);
}

.shipping {
  color:rgb(14, 141, 238);
  font-weight: bold;
}

.red {
  color: darkred;
}

.price {
  font-weight: bold;
  font-size: 1.2rem;
}

.flex-wrapper {
  display: flex;
}
</style>
