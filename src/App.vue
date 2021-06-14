<template>
	<div id="app">
		<h2>Оформление заказа</h2>
		<div class="order" v-if="!state.submitted">
			<div v-if="basketItems.length">
				<table class="basket-list">
					<tr>
						<th>Название</th>
						<th>Количество</th>
						<th>Цена</th>
						<th class="th-sum">Сумма</th>
						<th></th>
					</tr>
					<BasketItem 
						v-for="item in basketItems" 
						:key="item.id"
						:item="item"
						@change-quantity="changeQuantity"
						@delete-item="deleteItem"
					></BasketItem>
				</table>

				<h3>Данные к заказу</h3>
				<DataOrder 
					:form="form" 
					:deliveryPrice="deliveryPrice" 
					:total="total"
					:state="state"
					@sending-form="sendingForm"
					@submitted-form="submittedForm">
				</DataOrder>
			</div>
			<div v-else>Ваша корзина пуста</div>
		</div>
		<div class="order-submitted" v-else>Заказ оформлен!</div>
	</div>
</template>

<script>
import BasketItem from './components/BasketItem.vue'
import DataOrder from './components/DataOrder.vue'

export default {
	name: 'App',
	components: {
		BasketItem,
		DataOrder
	},
	data: function (){
		return {
			basketItems: [
				{
					"id": 343,
					"name": "Товар 1",
					"quantity": 1,
					"sum": 300
				},
				{
					"id": 12,
					"name": "Товар 2",
					"quantity": 4,
					"sum": 400
				},
				{
					"id": 1002,
					"name": "Товар 3",
					"quantity": 3,
					"sum": 550
				}
			],
			form:{
				name: "",
				phone: "",
				email: "",
				delivery: "delivery",
				address: "",
				coupon: "",
			},
			deliveryPriceValue: 300,
			state: {
				sending: false,
				submitted: false
			}
		}
	},
	methods:{
		changeQuantity: function (id, value) {
			var i = this.getItemIndex(id);
			if(i >= 0){
				this.basketItems[i].quantity = value;
			}
		},
		deleteItem: function (id) {
			var i = this.getItemIndex(id);
			if(i >= 0){
				this.basketItems.splice(i, 1);
			}
		},
		getItemIndex: function (id) {
			return this.basketItems.findIndex(function(v) {
				return v.id === id;
			});
		},
		sendingForm: function () {
			this.state.sending = true;
			this.state.submitted = false;
		},
		submittedForm: function () {
			this.state.sending = false;
			this.state.submitted = true;
		},
	},
	computed: {
		deliveryPrice: function () {
			return (this.form.delivery == "delivery") ? this.deliveryPriceValue : 0;
		},
		total: function () {
			var sum = 0;
			this.basketItems.forEach(function(item) {
				sum += item.quantity * item.sum;
			});
			sum += this.deliveryPrice;
			return sum;
		},
	},
}
</script>

<style>
	/* clear */
	p, img, h1, h2, h3, h4, h5, h6, div, table, a, img, form, iframe
	{
		margin:0;
		padding:0;
		border:0;
	}

	*{
		margin:0;
		padding:0;
		box-sizing: border-box;
	}
	table
	{
		border-collapse:collapse;
	}
	* :focus
	{
		outline:none;
	}
	/* ***** */

	@font-face {
		font-family: 'Geometria-Bold';
		src: url('./assets/fonts/Geometria-Bold.eot');
		src: url('./assets/fonts/Geometria-Bold.eot?#iefix') format('embedded-opentype'),
			url('./assets/fonts/Geometria-Bold.woff2') format('woff2'),
			url('./assets/fonts/Geometria-Bold.woff') format('woff'),
			url('./assets/fonts/Geometria-Bold.ttf') format('truetype'),
			url('./assets/fonts/Geometria-Bold.svg#Geometria-Bold') format('svg');
		font-weight: normal;
		font-style: normal;
	}
	@font-face {
		font-family: 'Geometria-Regular';
		src: url('./assets/fonts/Geometria-Regular.eot');
		src: url('./assets/fonts/Geometria-Regular.eot?#iefix') format('embedded-opentype'),
			url('./assets/fonts/Geometria-Regular.woff2') format('woff2'),
			url('./assets/fonts/Geometria-Regular.woff') format('woff'),
			url('./assets/fonts/Geometria-Regular.ttf') format('truetype'),
			url('./assets/fonts/Geometria-Regular.svg#Geometria-Regular') format('svg');
		font-weight: normal;
		font-style: normal;
	}
	#app {
		font-family: 'Geometria-Regular';
		font-size: 14px;
		width: 600px;
		padding: 30px 20px;
	}
	h2{
		font-size: 24px;
		line-height: 32px;
		font-family: 'Geometria-Bold';
		margin-bottom: 24px;
	}
	h3{
		font-size: 20px;
		line-height: 28px;
		font-family: 'Geometria-Bold';
		margin-bottom: 16px;
	}
	h4{
		font-family: 'Geometria-Bold';
		margin-bottom: 8px;
	}
	p{
		margin: 0;
	}
	b{
		font-family: 'Geometria-Bold';
	}
	.b-btn{
		width: 200px;
		text-align: center;
	}
		.btn{
			width: 100%;
			color: #FFF;
			background-color: #B49393;
			padding: 6px 14px;
			border-radius: 100px;
			border: none;
			cursor: pointer;
		}
		.order-btn{
			padding: 12px 24px;
			font-family: 'Geometria-Bold';
			font-size: 16px;
		}
	table{
		width: 100%;
		border-spacing: 0px;
		border-collapse: collapse;
		margin-bottom: 32px;
	}
		tr{
			border-bottom: 1px solid #000;
		}
			th{
				font-family: 'Geometria-Bold';
			}
			th.th-sum{
				min-width: 100px;
			}
			th, td{
				padding: 15px 8px;
				text-align: left;
			}
			table td input{
				width: 50px;
				margin: 0 6px;
				padding: 4px 10px;
				border-radius: 3px;
				border: 1px solid #000;
			}
			table td .quantity-btn{
				background-color: #FFF;
				border-radius: 100px;
				border: 1px solid #000;
				font-family: 'Geometria-Bold';
				font-size: 16px;
				width: 24px;
				height: 24px;
				cursor: pointer;
			}
			table tr td:first-child,
			table tr th:first-child{
				padding-left: 0;
			}
			table tr td:last-child,
			table tr th:last-child{
				width: 58px;
				padding-right: 0;
				text-align: right;
			}

	.preloader{
		width: 40px;
		height: 40px;
		margin: 0 auto;
	}
	.b-input{
		width: 250px;
		margin-bottom: 16px;
	}
		.b-input input{
			width: 100%;
			padding: 9px 16px;
			border-radius: 3px;
			border: 1px solid #000;
		}
		.b-input.error input{
			border: 1px solid #F00;
		}
		.error-message{
			color: red;
			font-size: 12px;
			display: none;
		}
		.b-input.error .error-message{
			display: block;
		}
	.b-radio-list{
		margin-bottom: 24px;
	}
		.b-radio{
			margin-bottom: 8px;
		}
			.b-radio input{
				margin: 0;
			}
			.b-radio label{
				margin-left: 8px;
			}
	.delivery-price{
		margin-bottom: 8px
	}
	.totals{
		font-size: 18px;
		margin-bottom: 16px;
	}
		
	.basket-empty{
		text-align: center;
	}
	.order-submitted{
		color: #1cbd0f;
		font-family: 'Geometria-Bold';
		font-size: 20px;
	}

</style>
