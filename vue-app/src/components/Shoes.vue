<template>

  <div class="product">
    <h1>Hello world</h1>
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
</template>

<script>
export default {
  name: 'Shoes',
  props: {
    msg: String,
    premium: {
      type: Boolean,
      required: true
    }
  },
  methods: {
    updateCart(id) {
      this.cart.push(id)
    },
    removeItem(id) {
      this.cart = this.cart.filter(item => {
        return item !== id
      })
    }
  }
}

</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
/* h3 {
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
} */
</style>
