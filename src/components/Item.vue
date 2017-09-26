<template>
<div class="row">
    <div class="col-md-4"><input class="form-control" @change="updateItem" v-model="item.name" placeholder="Item name"></div>
    <div class="col-md-2"><input class="form-control" @change="updateItem" v-model="item.quantity" placeholder="Quantity"></div>
    <div class="col-md-2"><input class="form-control" @change="updateItem" v-model="item.rate" placeholder="Rate"></div>
    <div class="col-md-2">{{ total }}</div>
    <div class="col-md-2"><button class="btn btn-danger" @click.prevent="deleteItem">Delete</button></div>
</div>
</template>

<script>
export default {
    name: 'item',
    props: [
      'itemObject'
    ],
    data() {
        return {
            item: this.itemObject
        }
    },
    computed: {
        total: function() {
            return (this.item.quantity * this.item.rate).toFixed(2)
        }
    },
    methods: {
        updateItem: function () {
            //get all the saved local items
            let items = JSON.parse(localStorage.getItem('items')) || [];

            // map through and find the one with same id as this one
            // then we set that item equal to our updated items and save
            // only downside is that this happens on ever change which isn't ideal
            // need to look at a way of throttling this.

            items.map((item) => {
                if (item.id === this.item.id) {
                    item.name = this.item.name;
                    item.quantity = this.item.quantity;
                    item.rate = this.item.rate;
                }
            });
            localStorage.setItem('items', JSON.stringify(items))
        },
        deleteItem() {
            this.$emit('delete', this.item.id)
        }
    }
}
</script>

<style>


</style>