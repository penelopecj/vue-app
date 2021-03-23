<template>
  <div class="product">
    <div class="product-image">
      <!-- v-bind omitted -->
      <img class="grey-box" :src="image" alt="redsox logo"/>
    </div>

    <div class="product-info">
      <h2>{{ brand }} {{ name }}</h2>
      <p v-if="inStock > 10">In Stock</p>
      <p v-else-if="inStock <= 10 && inStock > 0">Only {{ inStock }} left!</p>
      <p v-else class="red">Out of Stock</p>
      <p v-if="onSale" class="price">On SALE for just £{{ salePrice }}</p>
      <p v-else class="price">£{{ price }}</p>

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
          @mouseover="updateMePlease(index)"
          >
        </div>
      </div>

      <br />

      <!-- v-bind omitted -->
      <button @click="askToUpdate"
        :class="{ disabledBtn: inStock < 1 }"
      >Add 1 to Cart</button>
      <button v-on:click="askToRemove"
        :class="{ disabledBtn: cart < 1 }">Remove {{ variants[selectedVariant].variantColor }} {{ name }} from Cart</button>

      <hr />
      <h4>Top Reviews:</h4>

      <slot></slot>

    </div>
  </div>
</template>

<script>
export default {
  name: 'Product',
  props: {
    name: {
      type: String,
      default: 'Clothing'
    },
    brand: String,
    selectedVariant: Number,
    price: {
      type: Number,
      default: 19.99
    },
    onSale: {
      type: Boolean,
      default: false
    },
    salePrice: Number,
    details: Array,
    sizes: Array,
    variants: Array,
    inStock: Number,
    image: String,
    shipping: String,
    cart: Array
  },
  methods: {
    updateMePlease(index) {
      this.$emit('update-product', index)
    },
    askToUpdate() {
      this.$emit('update-cart', this.variants[this.selectedVariant].variantId)
    },
    askToRemove() {
      this.$emit('empty-cart', this.variants[this.selectedVariant].variantId)
    }
  }
}

</script>

<style scoped>

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

h4 {
  color: #2c3e50;
  font-size: 1.2rem;
}

img {
  width: 300px;
}

.color-box {
  width: 30px;
  height: 30px;
  margin-right: 5px;
  cursor: pointer;
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
</style>