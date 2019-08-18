<template>
  <div class="col-sm-12 col-md-6">
    <div class="card bg-light border-dark mb-3">
      <div 
        class="card-header"
            >{{ stock.name }}
        <small>(Price: {{ stock.price }} | Quantity: {{ stock.quantity }})</small>
      </div>
      <div class="card-body">
        <input
            v-model.number="quantity"
            type="number" 
            class="form-control" 
            placeholder="Quantity" 
            v-model="quantity"
            :class="{danger: insufficientQuantity}"
          />
        <button
            class="btn btn-success float-right"
            @click="sellStock"
            :disabled="insufficientQuantity || Number(quantity) <= 0 || !Number.isInteger(Number(quantity))"  
            >{{ insufficientQuantity ? 'Not enought Stocks' : 'Sell' }}</button>
      </div>
    </div>
  </div>
</template>

<script>
import {mapActions} from 'vuex'
export default {
    props: ['stock'],
    data() {
        return {
             quantity: 0
        }
    },
    computed: {
      insufficientQuantity() {
        return this.quantity > this.stock.quantity;
      }
    },
    methods: {
        ...mapActions({
            placeSellOrder: 'sellStock'
        }),
        sellStock() {
            const order = {
                stockId: this.stock.id,
                stockPrice: this.stock.price,
                quantity: this.quantity
            };
            this.placeSellOrder(order);
            this.quantity = 0;
        }
    },
}
</script>

<style>
    
</style>