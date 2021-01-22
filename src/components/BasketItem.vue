<template>
	<tr>
    <td>{{ item.name }}</td>
    <td>
      <button @click="reduceQuantity" class="quantity-btn">-</button>
        <input type="text" name="q" :value="quantity" @input="changeQuantity($event)">
      <button @click="addQuantity" class="quantity-btn">+</button></td>
    <td>{{ item.sum }} руб.</td>
    <td>{{ total }} руб.</td>
    <td><button @click="$emit('delete-item', item.id)" class="btn">Удалить</button></td>
  </tr>
</template>

<script>
export default {
	name: 'BasketItem',
	props: ['item'],
	data: function () {
		return {
			quantity: this.item.quantity
		}
	},
	computed: {
		total: function () {
			return this.quantity * this.item.sum;
		}
	},
	methods: {
		reduceQuantity: function () {
			this.quantity--;
			this.validateQuantity();
		},
		addQuantity: function () {
			this.quantity++;
			this.validateQuantity();
		},
		changeQuantity: function (event) {
			this.quantity = event.target.value;
			this.validateQuantity();
		},
		validateQuantity: function () {
			var _quantity = String(this.quantity);
			_quantity = _quantity.replace(/[^-\d]/g,'');
			_quantity = Number(_quantity);
			this.quantity = _quantity;
			if(_quantity < 1){
				this.quantity = 1;
			}
			if(_quantity > 99){
				this.quantity = 99;
			}
			this.$emit('change-quantity', this.item.id, this.quantity);
		},
	}
}
</script>

<style>

</style>