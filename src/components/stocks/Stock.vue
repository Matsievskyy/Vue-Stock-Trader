<template>
  <div class="col-sm-12 col-md-6">
    <div class="card bg-light mb-3">
      <div 
        class="card-header"
            >{{ stock.name }}
        <small>(Price: {{ stock.price }})</small>
      </div>
      <div class="card-body">
        <input
            v-model.number="quantity"
            type="number" 
            class="form-control" 
            placeholder="Quantity" 
            v-model="quantity"
            :class="{danger: insufficientFunds }"
        />
        <button
            class="btn btn-success float-right"
            @click="buyStock"
            :disabled="insufficientFunds || Number(quantity) <= 0 || !Number.isInteger(Number(quantity))"  
            >{{ insufficientFunds ? 'insufficient Funds' : 'Buy'  }}</button>
      </div>
    </div>
  </div>
</template>

<script>
export default {
    props: ['stock'],
    data() {
        return {
             quantity: 0
        }
    },
    methods: {
        buyStock() {
            const order = {
                stockId: this.stock.id,
                stockPrice: this.stock.price,
                quantity: this.quantity
            };
            this.$store.dispatch('buyStock', order);
            this.quantity = 0;
        },
    },
    computed: {
        funds() {
            return this.$store.getters.funds;
        },
        insufficientFunds() {
            return this.quantity * this.stock.price > this.funds;
        }
    }
}
</script>

<style>
    .form-control{
        width: 55%;
        display: inline;
        margin-right: 15px;
    }

    .card{
        width: 1000px;
    }

@media screen and (max-width: 1000px) {
    .card{
        width: 100%;
        }
    }

    .danger {
        border: 1px solid red;
    }

</style>