<template>
    <nav>    
      <div class="cart">
        <div class="flex-wrapper">
          <img class="logo" src="../images/favicon.png" alt="Vue logo" />
          <h3>ue Store</h3>
        </div>
        <p class="grey-box">Cart({{ cart.length }})</p>
      </div>
    </nav>
</template>

<script>

export default {
  name: 'Nav',
  props: ['cart']
}

</script>

<style scoped>
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
</style>