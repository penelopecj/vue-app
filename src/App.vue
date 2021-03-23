<template>
  <div id="app">
    <Nav :cart="cart"/>

    <Product 
      :name="products[0]"
      :brand="brands[0]"
      :selectedVariant="selectedVariant"
      :price="prices[0]"
      :onSale="onSale[0]"
      :salePrice="salePrices[0]"
      :details="details[0]"
      :sizes="sizes[0]"
      :variants="socksVariants"
      :inStock="inStock"
      :image="image"
      :cart="cart"
      @updateProduct="updateProduct"
      @updateCart="addToCart"
      @emptyCart="removeFromCart"
    />

    <Product 
      :name="products[1]"
      :brand="brands[1]"
      :selectedVariant="selectedShoeVariant"
      :price="prices[1]"
      :onSale="onSale[1]"
      :salePrice="salePrices[1]"
      :details="details[1]"
      :sizes="sizes[1]"
      :variants="shoesVariants"
      :inStock="shoeInStock"
      :image="shoeImage"
      :cart="cart"
      @updateProduct="updateShoeProduct"
      @updateCart="addShoeToCart"
      @emptyCart="removeShoeFromCart"
    />
  </div>

</template>



<script>
import redsox from './images/sox.png'
import bluesox from './images/bluesox.png'
import greenshoe from './images/shoe.png'
import purpleshoe from './images/purpleshoe.png'

import Nav from './components/Nav'
import Product from './components/Product'

export default {
  name: 'App',
  components: {
    Nav,
    Product
  },
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

.flex-wrapper {
  display: flex;
  align-items: center;
}

.grey-box {
  border: 1px solid rgb(196, 196, 196);
  text-align: center;
  margin: 5px;
  padding: 15px;
}
</style>
