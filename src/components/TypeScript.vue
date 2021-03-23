<template>
  <div class="typescript">
    <div class="grey-box">
      <h3>Your Cart</h3>
      <p>{{ cart.length }} items</p>
      <p>Total: £{{ calculateTotal() }}</p>
      <button>Proceed to Checkout</button>
    </div>
    
  </div>
</template>

<script lang="ts">

  export default {
    name: 'TypeScript',
    props: { 
      cart: Array,
      prices: Array,
      redSocks: Number,
      blueSocks: Number,
      greenShoes: Number,
      purpleShoes: Number,
    },
    methods: {
      calculateTotal() {
        const cartPrices: Number[] = []
        const socksPrice: Number = this.prices[0]
        const shoesPrice: Number = this.prices[1]

        this.cart.forEach(item => {
          if (item === this.redSocks || item === this.blueSocks) {
            cartPrices.push(socksPrice)
          } else {
            cartPrices.push(shoesPrice)
          }
        })

        const total = cartPrices.reduce((acc, curr) => {
          return acc + curr
        }, 0)

        return total.toFixed(2)

      }
    }
  }

</script>

<style scoped>

.grey-box {
  border-radius: 15px;
  padding: 0;
}

.typescript {
  width: 80%;
  max-width: 700px;
  margin: 0 auto;
  padding-bottom: 3rem;
}

h3 {
  background-color: rgba(0, 0, 0, 0.3);
  color: white;
  padding: 20px;
  margin-top: 0;
  border-radius: 13px 13px 0 0;
}

p {
  font-weight: bold;
}

button {
  color: white;
  font-weight: bold;
  font-size: 1rem;
  background: linear-gradient(rgb(135, 213, 96), teal);
  border: none;
  padding: 7px 20px;
  border-radius: 10px;
  cursor: pointer;
  margin-bottom: 20px;
}

button:hover {
  background: rgb(71, 155, 88);
}

</style>