<template>
  <div class="burger-item">
    <h4>{{ burger.name }}</h4>

    <img :src="burger.img" alt="hamburgare">

    <ul>
    <li v-if="burger.gluten">
      Contains <span class="allergene">gluten</span>
    </li>

    <li v-if="burger.lactose">
      Contains <span class="allergene">lactose</span>
    </li>


    <li>100% Beef</li>

    <div class="amount-row">
    Amount:
      <button @click="decrease">-</button>
      {{ amountOrdered }}
      <button @click="increase">+</button>
    </div>
    </ul>

  </div>
</template>

<script>
export default {
  name: 'OneBurger',
  props: {
    burger: Object
  },
  data() {
    return {
      amountOrdered: 0
    }
  },
  methods: {
    increase() {
      this.amountOrdered++

      this.$emit('orderedBurger', {
        name: this.burger.name,
        amount: this.amountOrdered
      })
    },
    decrease() {
      if (this.amountOrdered > 0) {
        this.amountOrdered--
      }
      this.$emit('orderedBurger', {
        name: this.burger.name,
        amount: this.amountOrdered
      })
    }
}
}

</script>

<style scoped>
  .allergene {
    font-weight: bold;
  }
  .burger-item > div {
    text-align: center;     
    margin: 8px;
    padding: 8px;
}

  .burger-item img {
    width: 100%;   
    height: auto;
    display: block;
    margin: auto;
}
  .burger-item h4 {
    font-size: 45px;       
    margin-bottom: 10px;
}
  .burger-item ul {
    font-size: 40px;       
    line-height: 1.2;      
}


.amount-row {
  font-size: 30px;
  margin-top: 10px;
}

.amount-row button {
  margin: 0 5px;
  font-size: 25px;
}
</style>