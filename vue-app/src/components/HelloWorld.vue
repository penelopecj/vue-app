<template>
  <div>
    <div class="cart">
      <p class="grey-box">Cart({{cart.length}})</p>
    </div>
    <div class="chart">
    </div>
    <!-- v-bind omitted -->
    <product :premium="premium"
      @add-to-cart="updateCart"
      @remove-from-cart="removeItem"></product>
    <shoes :premium="premium"
      @add-to-cart="updateCart"
      @remove-from-cart="removeItem"></shoes>
    <chart></chart> 
  </div>
</template>

<script>
export default {
  name: 'HelloWorld',
  props: {
    msg: String,
    premium: {
      type: Boolean,
      required: true
    }
  },
  template: `
    <div class="product">

    <div class="product-image">
      <!-- v-bind omitted -->
      <img class="grey-box" :src="image" />
    </div>

    <div class="product-info">
    <h1>{{ title }}</h1>
    <p v-if="inStock > 10">In Stock</p>
    <p v-else-if="inStock <= 10 && inStock > 0">Only {{inStock}} left!</p>
    <p v-else>Out of Stock</p>
    <p v-if="onSale">On SALE for just £{{salePrice}}</p>
    <p v-else>£{{price}}</p>

    <p>{{ shipping }} Shipping</p>

    <ul>
      <li v-for="detail in details">
          {{detail}}
        </li>
      </ul>

      <div class="size-select">
        <p>Please select a size:</p>
        <select>
          <option v-for="size in sizes">
            {{size}}
          </option>
        </select>
      </div>

      <p>Please select a color:</p>
      <!-- v-bind and v-on omitted -->
      <div v-for="(variant, index) in variants" 
        :key="variant.variantId"
        class="color-box"
        :style="{ backgroundColor: variant.variantColor}"
        @mouseover="updateProduct(index)">
      </div>

      <br />

      <!-- v-bind omitted -->
      <button v-on:click="addToCart"
        :class="{ disabledBtn: inStock < 1 }">Add 1 to Cart</button>
      <button v-on:click="removeFromCart"
        :class="{ disabledBtn: cart < 1 }">Remove {{ variants[selectedVariant].variantColor }} {{ product }} from Cart</button>

      </div>
      </div>
    `,
  data() {
    return {
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
          variantImage: './sox.png',
          variantQuantity: 7
        },
        {
          variantId: 2235,
          variantColor: 'Blue',
          variantImage: './bluesox.png',
          variantQuantity: 11
        },
      ],
    }
  },
  methods: {
    addToCart() {
      if (this.variants[this.selectedVariant].variantQuantity > 0) {
        this.$emit('add-to-cart', this.variants[this.selectedVariant].variantId)
        this.variants[this.selectedVariant].variantQuantity -= 1
      }
    },
    removeFromCart() {
      this.$emit('remove-from-cart', this.variants[this.selectedVariant].variantId)
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
  }
}

//   Vue.component('shoes', {
//     props: {
//       premium: {
//         type: Boolean,
//         required: true
//       }
//     },
//     template: `
//       <div class="product">

//       <div class="product-image">
//         <!-- v-bind omitted -->
//         <img class="grey-box" :src="image" />
//       </div>

//       <div class="product-info">
//       <h1>{{ title }}</h1>
//       <p v-if="inStock > 10">In Stock</p>
//       <p v-else-if="inStock <= 10 && inStock > 0">Only {{inStock}} left!</p>
//       <p v-else>Out of Stock</p>
//       <p v-if="onSale">On SALE for just £{{salePrice}}</p>
//       <p v-else>£{{price}}</p>

//       <p>{{ shipping }} Shipping</p>

//       <ul>
//         <li v-for="detail in details">
//           {{detail}}
//         </li>
//       </ul>

//       <div class="size-select">
//         <p>Please select a size:</p>
//         <select>
//           <option v-for="size in sizes">
//             {{size}}
//           </option>
//         </select>
//       </div>

//       <p>Please select a color:</p>
//       <!-- v-bind and v-on omitted -->
//       <div v-for="(variant, index) in variants" 
//         :key="variant.variantId"
//         class="color-box"
//         :style="{ backgroundColor: variant.variantColor}"
//         @mouseover="updateProduct(index)">
//       </div>

//       <br />

//       <!-- v-bind omitted -->
//       <button v-on:click="addToCart"
//         :class="{ disabledBtn: inStock < 1 }">Add 1 to Cart</button>
//       <button v-on:click="removeFromCart"
//         :class="{ disabledBtn: cart < 1 }">Remove {{ variants[selectedVariant].variantColor }} {{ product }} from Cart</button>

//       </div>
//       </div>
//     `,
//     data() {
//       return {
//         product: 'Shoes',
//         brand: 'Comfort Casual',
//         selectedVariant: 0,
//         price: 35.77,
//         onSale: false,
//         salePrice: 17.38,
//         details: ['rubber soles', 'water-resistent', 'arch support'],
//         sizes: ['4', '5', '6', '7', '8', '9', '10', '11', '12'],
//         variants: [
//           {
//             variantId: 2236,
//             variantColor: 'Green',
//             variantImage: './shoe.png',
//             variantQuantity: 50
//           },
//           {
//             variantId: 2237,
//             variantColor: 'Purple',
//             variantImage: './purpleshoe.png',
//             variantQuantity: 5
//           },
//         ],
//       }
//     },
//     methods: {
//       addToCart() {
//         if (this.variants[this.selectedVariant].variantQuantity > 0) {
//           this.$emit('add-to-cart', this.variants[this.selectedVariant].variantId)
//           this.variants[this.selectedVariant].variantQuantity -= 1
//         }
//       },
//       removeFromCart() {
//         this.$emit('remove-from-cart', this.variants[this.selectedVariant].variantId)
//         //this.variants[this.selectedVariant].variantQuantity += 1
//       },
//       updateProduct(index) {
//         this.selectedVariant = index
//       }
//     },
//     computed: {
//       title() {
//         return this.brand + ' ' + this.product
//       },
//       image() {
//         return this.variants[this.selectedVariant].variantImage
//       },
//       inStock() {
//         return this.variants[this.selectedVariant].variantQuantity
//       },
//       shipping() {
//         if (this.premium) {
//           return "Free" 
//         } 
//         return `£${2.99}`
//       }
//     }
//   })

//   Vue.component('chart', {
//     template: `
//       <div class="chart">
//         Chart goes here.
//       </div>
//     `
//   })


//   var app = new Vue({
//     el: '#app',
//     data: {
//       premium: true,
//       cart: [],
//     },
//     methods: {
//       updateCart(id) {
//         this.cart.push(id)
//       },
//       removeItem(id) {
//         // this.cart.forEach(item => {
//         //   if (item === id) variants[selectedVariant].variantQuantity += 1
//         // })
//         this.cart = this.cart.filter(item => {
//           return item !== id
//         })
//       }
//     }
//   })

//   console.log(app)

</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
h3 {
  margin: 40px 0 0;
}
ul {
  list-style-type: none;
  padding: 0;
}
li {
  display: inline-block;
  margin: 0 10px;
}
a {
  color: #42b983;
}
</style>
