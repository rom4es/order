<template>
	<form action="" method="post" @submit.prevent="submitForm">
		<div class="b-input" :class="{error: validate.name.error}">
			<input 
				type="text" 
				name="name" 
				placeholder="Имя*" 
				v-model="form.name" 
				@input="resetError('name')">
			<p class="error-message">{{validate.name.errorMsg}}</p>
		</div>
		<div class="b-input" :class="{error: validate.phone.error}">
			<masked-input 
				type="text" 
				name="phone" 
				placeholder="Телефон*" 
				mask="\+\7 (111) 111-11-11" 
				v-model="form.phone" 
				@input="resetError('phone')"/>
			<p class="error-message">{{validate.phone.errorMsg}}</p>
		</div>
		<div class="b-input" :class="{error: validate.email.error}">
			<input 
				type="text" 
				name="email" 
				placeholder="Email*" 
				v-model="form.email" 
				@input="resetError('email')">
			<p class="error-message">{{validate.email.errorMsg}}</p>
		</div>
		<h4>Способ доставки</h4>
		<div class="b-radio-list">
			<div class="b-radio">
				<input 
					id="ladel-delivery" 
					type="radio" 
					name="delivery" 
					value="delivery" 
					v-model="form.delivery">
				<label for="ladel-delivery">Доставка по городу</label>
			</div>
			<div class="b-radio">
				<input 
					id="ladel-pickup" 
					type="radio" 
					name="delivery" 
					value="pickup" 
					v-model="form.delivery"
					@change="resetError('address')">
				<label for="ladel-pickup">Самовывоз</label>
			</div>
			<div class="b-input" v-show="form.delivery == 'delivery'" :class="{error: validate.address.error}">
				<input 
					type="text" 
					name="address" 
					placeholder="Адрес доставки*" 
					v-model="form.address" 
					:disabled="form.delivery == 'pickup'"
					@input="resetError('address')">
				<p class="error-message">{{validate.address.errorMsg}}</p>
			</div>
		</div>

		<h4 class="delivery-price">Стоимость доставки: {{ deliveryPriceText() }}</h4>
		<h4 class="totals">Общая сумма: {{ total }} руб.</h4>

		<div class="b-btn">
			<button v-if="!state.sending" class="btn order-btn">Оформить заказ</button>
			<img class="preloader" src="@/assets/i/preloader.svg" v-else>
		</div>

	</form>
</template>

<script>
import MaskedInput from 'vue-masked-input'

export default {
	name: 'DataOrder',
	components:{
		MaskedInput,
	},
	data: function () {
		return {
			validate: {
				name: {
					error: false,
					errorMsg: ""
				},
				phone: {
					error: false,
					errorMsg: ""
				},
				email: {
					error: false,
					errorMsg: ""
				},
				address: {
					error: false,
					errorMsg: ""
				},
			},
		}
	},
	props: ['form', 'deliveryPrice', 'total', 'state'],
	methods: {
		deliveryPriceText: function () { 
			return (this.deliveryPrice > 0) ? this.deliveryPrice+" руб." : "бесплатно";
		},
		submitForm: function () {
			let _this = this;
			let fields = {
				name: this.form.name,
				phone: this.form.phone,
				email: this.form.email,
				address: this.form.address,
			};

			let valid = true;
			for (let key in fields){
				if(key == "address"){
					if(!fields[key] && _this.form.delivery == "delivery"){
						this.setError(key, "Поле обязательное");
						valid = false;
					}
				}else{
					if(!fields[key]){
						this.setError(key, "Поле обязательное");
						valid = false;
					}
				}
				if(key == "email" && fields[key] && !_this.validEmail(fields[key])){
					this.setError(key, "Неверный email");
					valid = false;
				}
			}

			if(valid){
				//типо ajax-запрос
				_this.$emit('sending-form');
				setTimeout(function () {
					_this.$emit('submitted-form');
				}, 2000);
			}
			
		},
		validEmail: function (email) {
			var re = /^(([^<>()[\]\\.,;:\s@"]+(\.[^<>()[\]\\.,;:\s@"]+)*)|(".+"))@((\[[0-9]{1,3}\.[0-9]{1,3}\.[0-9]{1,3}\.[0-9]{1,3}\])|(([a-zA-Z\-0-9]+\.)+[a-zA-Z]{2,}))$/;
			return re.test(email);
		},
		setError: function (field, msg) {
			this.validate[field].error = true;
			this.validate[field].errorMsg = msg;
		},
		resetError: function (field) {
			this.validate[field].error = false;
		},
	},
}
</script>

<style>
	
</style>