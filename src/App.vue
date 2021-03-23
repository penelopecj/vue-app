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
      :shipping="shipping"
      :cart="cart"
      @update-product="updateProduct"
      @update-cart="addToCart"
      @empty-cart="removeFromCart"
    > 
      <ul class="reviews">
        <li v-for="review in reviews[0]" v-bind:key="review.id">
          "{{ review }}"
        </li>
      </ul>
    </Product>

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
      :shipping="shipping"
      :cart="cart"
      @update-product="updateShoeProduct"
      @update-cart="addShoeToCart"
      @empty-cart="removeShoeFromCart"
    > 
      <ul class="reviews">
        <li v-for="review in reviews[1]" v-bind:key="review.id">
          "{{ review }}"
        </li>
      </ul>
    </Product>

    <TypeScript />
  </div>

</template>



<script>
import redsox from './images/sox.png'
import bluesox from './images/bluesox.png'
import greenshoe from './images/shoe.png'
import purpleshoe from './images/purpleshoe.png'

import Nav from './components/Nav'
import Product from './components/Product'
import TypeScript from './components/TypeScript'

export default {
  name: 'App',
  components: {
    Nav,
    Product,
    TypeScript
  },
  data() {
    return {
      premium: true,
      cart: [],
      products: ['Socks', 'Shoes'],
      reviews: [
        [
          'Go sox!',
          'I love how these two colours go with everything.',
          'Keep my feet nice and warm!',
          'Great with sandals.'
        ],
        [
          'The best shoes ever!',
          'Really stylish, my kids were impressed.'
        ]
      ],
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
      const selectedSocks = this.socksVariants[this.selectedVariant]

      if (selectedSocks.variantQuantity > 0) {
        this.cart.push(selectedSocks.variantId)
        selectedSocks.variantQuantity -= 1
      }
    },
    removeFromCart() {
      const selectedSocksInCart = []
      const selectedSocks = this.socksVariants[this.selectedVariant]
      const selectedSocksId = this.socksVariants[this.selectedVariant].variantId

      this.cart = this.cart.filter(item => {
        if (item === selectedSocksId) {
          selectedSocksInCart.push(item)
        }
        return item !== selectedSocksId
      })

      selectedSocks.variantQuantity += selectedSocksInCart.length
    },
    updateProduct(index) {
      this.selectedVariant = index
    },
    addShoeToCart() {
      const selectedShoes = this.shoesVariants[this.selectedShoeVariant]

      if (selectedShoes.variantQuantity > 0) {
        this.cart.push(selectedShoes.variantId)
        selectedShoes.variantQuantity -= 1
      }
    },
    removeShoeFromCart() {
      const selectedShoesInCart = []
      const selectedShoes = this.shoesVariants[this.selectedShoeVariant]
      const selectedShoesId = this.shoesVariants[this.selectedShoeVariant].variantId

      this.cart = this.cart.filter(item => {
        if (item === selectedShoesId) {
          selectedShoesInCart.push(item)
        }
        return item !== selectedShoesId
      })

      selectedShoes.variantQuantity += selectedShoesInCart.length
    },
    updateShoeProduct(index) {
      this.selectedShoeVariant = index
    }
  },
  computed: {
    image() {
      const selectedImage = this.socksVariants[this.selectedVariant].variantImage
      return selectedImage
    },
    inStock() {
      const selectedProductQuantity = this.socksVariants[this.selectedVariant].variantQuantity
      return selectedProductQuantity
    },
    shipping() {
      const userHoldsAPremiumAccount = this.premium
      if (userHoldsAPremiumAccount) {
        return "Free" 
      } 
      return `£${2.99}`
    },
    shoeImage() {
      const selectedImage = this.shoesVariants[this.selectedShoeVariant].variantImage
      return selectedImage
    },
    shoeInStock() {
      const selectedProductQuantity = this.shoesVariants[this.selectedShoeVariant].variantQuantity
      return selectedProductQuantity
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

.reviews {
  list-style: none;
  padding-left: 0;
}

.reviews li {
  margin-bottom: 10px;
  border: 1px solid rgb(210, 210, 210);
  padding: 10px;
  border-radius: 10px;
  color: rgb(79, 79, 79);
}
</style>
