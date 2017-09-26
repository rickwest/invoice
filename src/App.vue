<template>
    <div id="app">
        <div class="container">

            <item v-if="items" v-for="item in items" :item-object="item" :key="item.id" v-on:delete="deleteItem" />
            <div class="row">
                <div class="col-md-12">
                    <button class="btn btn-success" @click.prevent="newItem" >Add a new item</button>
                </div>
            </div>
            <div class="row">
                <h1>Grand Total: {{ grandTotal }}</h1>
                <h1>Cis: {{ cis }}</h1>
                <h1>Total Payable: {{ netTotal }}</h1>

            </div>
        </div>
    </div>
</template>

<script>
import Item from './components/Item.vue';

export default {
    name: 'items',
    components: {
        item: Item
    },
    data () {
        return {
            items: JSON.parse(localStorage.getItem('items')) || [],
            total: 0
        }
    },
    methods: {
        newItem: function() {
            this.items.unshift({
                id: Date.now(),
                name: '',
                rate: '',
                quantity: '',
          });
          localStorage.setItem('items', JSON.stringify(this.items))
      },
        deleteItem: function (id) {
            this.items = this.items.filter((item) => {
                return item.id !== id
            });
            localStorage.setItem('items', JSON.stringify(this.items))
        }
  },
    computed: {
        grandTotal: function() {
            let grandTotal = 0;
            this.items.map((item) => {
                grandTotal += (item.quantity * item.rate)
            });
            return grandTotal.toFixed(2);
        },
        cis: function() {
            return (this.grandTotal * 0.2).toFixed(2)
        },
        netTotal: function() {
            return (this.grandTotal * 0.8).toFixed(2);
        }
    }
}
</script>

<style>
    .row {
        margin-top: 20px;
    }
</style>
