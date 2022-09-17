<template>
  <b-container fluid class="my-4">
    <b-row>
      <b-col cols="6">
        <h4>Food</h4>
        <table class="table">
          <thead>
          <tr>
            <th scope="col">SL</th>
            <th scope="col">Food</th>
            <th scope="col">Price</th>
          </tr>
          </thead>
          <tbody>
          <tr v-for="(food, i) in foods" :key="i">
            <td>{{ i + 1}}</td>
            <td @click="addFoodToOrder(food)" style="cursor: pointer">{{ food.Food }}</td>
            <td>{{ food.price }}</td>
          </tr>
          </tbody>
        </table>
      </b-col>
      <b-col cols="6">
        <H4>Order</H4>
        <table class="table">
          <thead>
          <tr>
            <th scope="col">Food Name</th>
            <th scope="col">Quantity</th>
            <th scope="col">Subtotal</th>
            <th scope="col">Action</th>
          </tr>
          </thead>
          <tbody>
          <tr v-for="(order, i) in orders" :key="i">
            <td >{{ order.name }}</td>
            <td>
              <b-button variant="primary" :disabled="order.quantity <= 1"  size="sm" @click="decrease(i)">-</b-button>
              <span class="mx-3">{{ order.quantity }}</span>
              <b-button variant="primary"  size="sm" @click="increase(i)">+</b-button>
            </td>
            <td>{{ order.quantity * order.price }}</td>
            <td><b-button variant="danger" size="sm" @click="deleteOrder(i)">Delete</b-button></td>
          </tr>
          <tr v-if="orders && orders.length">
            <td colspan="2">Sub Total</td>
            <td colspan="2">{{ SubTotal }}</td>
          </tr>
          <tr v-if="orders && orders.length">
            <td colspan="2">Discount</td>
            <td colspan="1">
              <b-form-input
                  v-model="discount"
                  placeholder="Enter Discount %"
                  required
              ></b-form-input>
            </td>
            <td >{{ discountTotal }}</td>
          </tr>
          <tr v-if="orders && orders.length">
            <td colspan="2">Vat</td>
            <td colspan="1">
              <b-form-input
                  v-model="vat"
                  placeholder="Enter Vat %"
                  required
              ></b-form-input>
            </td>
            <td colspan="2">{{ vatTotal }}</td>
          </tr>
          <tr v-if="orders && orders.length">
            <td colspan="2">Final Total</td>
            <td colspan="2">{{ finalTotal }}</td>
          </tr>
          </tbody>
        </table>
      </b-col>
    </b-row>
  </b-container>
</template>

<script>
export default {
  name: 'App',
  data() {
    return {
      discount: 0,
      vat: 0
    }
  },
  computed:{
    foods(){
      return this.$store.getters.foods
    },
    orders(){
      return this.$store.getters.orders
    },
    SubTotal(){
      if(this.orders && this.orders.length) {
        return this.orders.map(item => item.price * item.quantity).reduce((prev, next) => prev + next)
      } else {
        return 0
      }
    },
    discountTotal(){
      if(this.orders && this.orders.length) {
        return (this.SubTotal * this.discount) / 100
      } else {
        return 0
      }
    },
    vatTotal(){
      if(this.orders && this.orders.length) {
        return ((this.SubTotal - this.discountTotal) * this.vat) / 100
      } else {
        return 0
      }
    },
    finalTotal(){
      if(this.orders && this.orders.length) {
        return ((this.SubTotal - this.discountTotal) + this.vatTotal)
      } else {
        return 0
      }
    },
  },
  methods:{
    addFoodToOrder(food){
      this.$store.commit("ADD_FOOD", food)
    },
    deleteOrder(index){
      this.$store.commit("DELETE_ORDER", index)
    },
    increase(index){
      this.$store.commit("INCREASE_QUANTITY", index)
    },
    decrease(index){
      this.$store.commit("DECREASE_QUANTITY", index)
    }
  }
}
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>
