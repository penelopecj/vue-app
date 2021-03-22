<template>
  <div id="app">
    <nav>    
      <div class="cart">
        <div class="flex-wrapper">
          <img class="logo" src="./images/favicon.png" alt="Vue logo" />
          <h3>ue Store</h3>
        </div>
        <p class="grey-box">Cart({{cart.length}})</p>
      </div>
    </nav>

    <!-- SOCKS -->
    <div class="product">
      <div class="product-image">
        <!-- v-bind omitted -->
        <img class="grey-box" :src="image" alt="redsox logo"/>
      </div>

      <div class="product-info">
        <h2>{{ brands[0] }} {{ products[0] }}</h2>
        <p v-if="inStock > 10">In Stock</p>
        <p v-else-if="inStock <= 10 && inStock > 0">Only {{inStock}} left!</p>
        <p v-else class="red">Out of Stock</p>
        <p v-if="onSale[0]" class="price">On SALE for just £{{salePrices[0]}}</p>
        <p v-else class="price">£{{prices[0]}}</p>

        <p class="shipping">{{ shipping }} Shipping</p>

        <ul>
          <li v-for="detail in details[0]" :key="detail.id">
            {{detail}}
          </li>
        </ul>

        <div class="size-select">
          <p>Please select a size:</p>
          <select>
            <option v-for="size in sizes[0]" :key="size.id">
              {{size}}
            </option>
          </select>
        </div>

        <p>Please select a color:</p>
        <!-- v-bind and v-on omitted -->
        <div class="flex-wrapper">
          <div v-for="(variant, index) in socksVariants" 
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
          :class="{ disabledBtn: cart < 1 }">Remove {{ socksVariants[selectedVariant].variantColor }} {{ product }} from Cart</button>
      </div>
    </div>

    <!-- SHOES -->
    <div class="product">
      <div class="product-image">
        <!-- v-bind omitted -->
        <img class="grey-box" :src="shoeImage" alt="shoes"/>
      </div>

      <div class="product-info">
        <h2>{{ brands[1] }} {{ products[1] }}</h2>
        <p v-if="shoeInStock > 10">In Stock</p>
        <p v-else-if="shoeInStock <= 10 && shoeInStock > 0">Only {{shoeInStock}} left!</p>
        <p v-else class="red">Out of Stock</p>
        <p v-if="onSale[1]" class="price">On SALE for just £{{salePrices[1]}}</p>
        <p v-else class="price">£{{prices[1]}}</p>

        <p class="shipping">{{ shipping }} Shipping</p>

        <ul>
          <li v-for="detail in details[1]" :key="detail.id">
            {{detail}}
          </li>
        </ul>

        <div class="size-select">
          <p>Please select a size:</p>
          <select>
            <option v-for="size in sizes[1]" :key="size.id">
              {{size}}
            </option>
          </select>
        </div>

        <p>Please select a color:</p>
        <!-- v-bind and v-on omitted -->
        <div class="flex-wrapper">
          <div v-for="(variant, index) in shoesVariants" 
            :key="variant.variantId"
            class="color-box"
            :style="{ backgroundColor: variant.variantColor}"
            @mouseover="updateShoeProduct(index)">
          </div>
        </div>

        <br />

        <!-- v-bind omitted -->
        <button v-on:click="addShoeToCart"
          :class="{ disabledBtn: shoeInStock < 1 }">Add 1 to Cart</button>
        <button v-on:click="removeShoeFromCart"
          :class="{ disabledBtn: cart < 1 }">Remove {{ shoesVariants[selectedShoeVariant].variantColor }} {{ product }} from Cart</button>
      </div>
    </div>
  
  </div>

</template>



<script>
import redsox from './images/sox.png'
import bluesox from './images/bluesox.png'
import greenshoe from './images/shoe.png'
import purpleshoe from './images/purpleshoe.png'

export default {
  name: 'App',
  data() {
    return {
      premium: true,
      cart: [],
      products: ['Socks', 'Shoes'],
      brands: ['Boston Sports', 'Comfort Casual'],
      selectedVariant: 0,
      selectedShoeVariant: 0,
      prices: [19.99, 35.77],
      onSale: [true, false],
      salePrices: [9.99, 17.38],
      details: [['80% cotton', '20% polyester', 'one-size-fits-all'], ['rubber soles', 'water-resistent', 'arch support']],
      sizes: [['S', 'M', 'L'], ['4', '5', '6', '7', '8', '9', '10', '11', '12']],
      socksVariants: [
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
      shoesVariants: [
        {
          variantId: 2236,
          variantColor: 'Green',
          variantImage: greenshoe,
          variantQuantity: 50
        },
        {
          variantId: 2237,
          variantColor: 'Purple',
          variantImage: purpleshoe,
          variantQuantity: 5
        },
      ]
    }
  },
  methods: {
    addToCart() {
      if (this.socksVariants[this.selectedVariant].variantQuantity > 0) {
        this.cart.push(this.socksVariants[this.selectedVariant].variantId)
        this.socksVariants[this.selectedVariant].variantQuantity -= 1
      }
    },
    removeFromCart() {
      this.cart = this.cart.filter(item => {
        return item !== this.socksVariants[this.selectedVariant].variantId
      })
      //this.socksVariants[this.selectedVariant].variantQuantity += 1
    },
    updateProduct(index) {
      this.selectedVariant = index
    },
    addShoeToCart() {
      if (this.shoesVariants[this.selectedShoeVariant].variantQuantity > 0) {
        this.cart.push(this.shoesVariants[this.selectedShoeVariant].variantId)
        this.shoesVariants[this.selectedShoeVariant].variantQuantity -= 1
      }
    },
    removeShoeFromCart() {
      this.cart = this.cart.filter(item => {
        return item !== this.shoesVariants[this.selectedShoeVariant].variantId
      })
      //this.socksVariants[this.selectedShoeVariant].variantQuantity += 1
    },
    updateShoeProduct(index) {
      this.selectedShoeVariant = index
    }
  },
  computed: {
    image() {
      return this.socksVariants[this.selectedVariant].variantImage
    },
    inStock() {
      return this.socksVariants[this.selectedVariant].variantQuantity
    },
    shipping() {
      if (this.premium) {
        return "Free" 
      } 
      return `£${2.99}`
    },
    shoeImage() {
      return this.shoesVariants[this.selectedShoeVariant].variantImage
    },
    shoeInStock() {
      return this.shoesVariants[this.selectedShoeVariant].variantQuantity
    },
    shoeShipping() {
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
  font-family: Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  margin: 0;
}

body {
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
  padding: 5px 0;
}

.logo {
  height: 30px;
  width: 30px;
}

.product {
  display: flex;
  justify-content: space-evenly;
  padding: 20px;
  margin-top: 5rem;
}

h2 {
  color: #2c3e50;
  font-size: 2rem;
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
  color: yellowgreen; 
  padding-right: 20px;
  font-size: 1.7rem;
  margin: 0;
  text-transform: uppercase;
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

.disabledBtn:hover {
  background-color: rgb(196, 196, 196);
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
  align-items: center;
}
</style>
