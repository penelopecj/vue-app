<template>
  <div class="typescript">
    <div class="grey-box">
      <h3>Your Cart</h3>
      <p class="total">Total Items: {{ cart.length }}</p>
      <p>{{ items()[0] }} x {{ red }} {{ brands[0] }} {{ names[0] }}</p>
      <p>{{ items()[1] }} x {{ red }} {{ brands[0] }} {{ names[0] }}</p>
      <p>{{ items()[2] }} x {{ red }} {{ brands[1] }} {{ names[1] }}</p>
      <p>{{ items()[3] }} x {{ red }} {{ brands[1] }} {{ names[1] }}</p>
      <p class="total">Total Price: £{{ calculateTotal() }}</p>
      <button>Proceed to Checkout</button>
    </div>
    
  </div>
</template>

<script lang="ts">

  export default {
    name: 'TypeScript',
    props: { 
      cart: Array,
      names: Array,
      brands: Array,
      prices: Array,
      redSocksId: Number,
      blueSocksId: Number,
      greenShoesId: Number,
      purpleShoesId: Number,
      red: String,
      blue: String,
      green: String,
      purple: String,
    },
    methods: {
      items() {
        let redSocksCount: number = 0
        let blueSocksCount: number = 0
        let greenShoesCount: number = 0
        let purpleShoesCount: number = 0

        this.cart.forEach(item => {
          if (item === this.redSocksId) {
            redSocksCount += 1
          } else if (item === this.blueSocksId) {
            blueSocksCount += 1
          } else if (item === this.greenShoesId) {
            greenShoesCount += 1
          } else {
            purpleShoesCount += 1
          }
        })

        return [redSocksCount, blueSocksCount, greenShoesCount, purpleShoesCount]
      },
      calculateTotal() {
        const cartPrices: number[] = []
        const socksPrice: number = this.prices[0]
        const shoesPrice: number = this.prices[1]

        this.cart.forEach(item => {
          if (item === this.redSocksId || item === this.blueSocksId) {
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

.total {
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