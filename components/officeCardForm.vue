<template>
	<div class="form bg-white rounded-lg shadow-lg px-6 pt-5 pb-5">
		<div class="flex justify-between align-middle mb-8">
			<h4 class="font-semibold darkblue">{{isNew ? 'New' : 'Edit' }} Location</h4>
			<a href="#" class="self-center" @click.prevent="closeForm"><img src="@/assets/img/close.png"/></a>
		</div>

		<form name="cardForm" @submit.prevent="submitCard">

			<div class="rounded-lg shadow-lg mb-6" :class="formModel.color ? `card-${formModel.color}`: 'card-red'">
				<a href="#" class="flex justify-center outline-none py-12" @click.prevent='showCardColorOptions = !showCardColorOptions'>
					<div>
						<h4 class="font-normal text-white text-md mr-4">Select Color</h4>
					</div>
					<img src="@/assets/img/chevron-sm.png" class="self-center" :style="{transition: 'all 300ms ease-in-out', transform: showCardColorOptions ? 'rotate(0deg)' : 'rotate(180deg)' }" />
				</a>
			</div>

			<transition-group>
				<div v-for="(cardColor, cardColorName) in cardColors"
					v-show="showCardColorOptions"
					:key="cardColorName"
					class="rounded-lg mb-2"
					:class="`card-${cardColorName}`">
					<a href="#" class="flex justify-center outline-none py-12" @click.prevent="formModel.color=cardColorName; showCardColorOptions = false">
						<img src="@/assets/img/checked.png" class="self-center" v-show="formModel.color==cardColorName" />&nbsp;
					</a>
				</div>
			</transition-group>

			<p>
				<label>Title <span>*</span></label>
				<input type="text" name="title" v-model="formModel.title" :class="{'invalid':formErrors.title}">
				<small v-show="formErrors.title">{{formErrors.title}}</small>
			</p>
			<p>
				<label>Enter the address <span>*</span></label>
				<input type="text" name="address" v-model="formModel.address" :class="{'invalid':formErrors.address}">
				<small v-show="formErrors.address">{{formErrors.address}}</small>
			</p>

			<h5 class="text-blue border border-t-0 border-r-0 border-l-0 border-gray-200 border-solid pb-2 font-hairline text-xs mb-6 mt-8">
				CONTACT INFORMATION</h5>

			<p>
				<label>Full name <span>*</span></label>
				<input type="text" name="fullName" v-model="formModel.fullName" :class="{'invalid':formErrors.fullName}">
				<small v-show="formErrors.fullName">{{formErrors.fullName}}</small>
			</p>
			<p>
				<label>Job Position <span>*</span></label>
				<input type="text" name="jobPosition" v-model="formModel.jobPosition" :class="{'invalid':formErrors.jobPosition}">
				<small v-show="formErrors.jobPosition">{{formErrors.jobPosition}}</small>
			</p>
			<p>
				<label>Email address <span>*</span></label>
				<input type="email" name="email" v-model="formModel.email" :class="{'invalid':formErrors.email}">
				<small v-show="formErrors.email">{{formErrors.email}}</small>
			</p>
			<p>
				<label>Phone <span>*</span></label>
				<input type="text" name="phone" v-model="formModel.phone" :class="{'invalid':formErrors.phone}">
				<small v-show="formErrors.phone">{{formErrors.phone}}</small>
			</p>
			<p class="mb-0">
				<input type="submit" value="Save" class="py-2 px-5 rounded text-white font-hairline cursor-pointer outline-none" :disabled="!saveButton">
			</p>
		</form>

	</div>
</template>

<script>
export default {
	data: function() {
		return {
			formErrors: {},
			formModel: {},
			saveButton: false,
			isNew: true,
			showCardColorOptions: false
		}
	},
	props: {
		cardData: {
			type: Object,
	 		default: null
		 },
		 cardColors: {
			type: Object
		 },
	},
	methods: {
		submitCard() {
			$nuxt.$emit('submitCard', this.formModel);
			this.closeForm();
		},
		closeForm() {
			if(this.isNew) {
				this.formModel = {"color":"red"};
				this.formErrors = {};
				this.saveButton = false;
				this.showCardColorOptions = false;
			} else {
				this.formModel = {...this.cardData};
			}
			$nuxt.$emit('closeForm');
		},
		validateForm(formObj) {
			let inputQty = this.$el.querySelectorAll('form input:not([type="submit"])').length;

			for(let input in formObj) {
				let inputName = input;
				let inputValue = formObj[input];
				let inputType = (this.$el.querySelector('input[name="'+inputName+'"]')) ? this.$el.querySelector('input[name="'+inputName+'"]').type : null;

				delete this.formErrors[inputName];

				if(inputType != null) {
					if(inputValue === "" ) {
						this.formErrors = {[inputName]: 'This field cannot be empty', ...this.formErrors};
					}

					if (inputName == "email" && !(/^(([^<>()\[\]\\.,;:\s@"]+(\.[^<>()\[\]\\.,;:\s@"]+)*)|(".+"))@((\[[0-9]{1,3}\.[0-9]{1,3}\.[0-9]{1,3}\.[0-9]{1,3}])|(([a-zA-Z\-0-9]+\.)+[a-zA-Z]{2,}))$/.test(inputValue))) {
						this.formErrors = {[inputName]: 'This email address is invalid', ...this.formErrors};
					}
				}
				this.saveButton = ((Object.keys(formObj).length-(this.isNew?1:2) == inputQty) && !Object.keys(this.formErrors).length);
			}
		},
	},
	watch: {
		cardData: {
			immediate:true,
			handler: function(externalCardData) {
				if(externalCardData){
					this.isNew = false;
					this.saveButton = true;
					this.formModel = {...externalCardData};
				} else {
					this.formModel = {"color":"red"};
				}
			}
		},
		formModel: {
			deep:true,
			handler: function(formObj) {
				this.validateForm(formObj);
			}
		}
	}
};
</script>

<style>
	form p {
		@apply mb-6 relative
	}

	label {
		color: #313E4F;
		@apply block font-light mb-1;
	}

	label span {
		@apply text-gray-500 text-sm font-hairline
	}

	input[type="text"],
	input[type="email"] {
		font-size: 14px;
		@apply rounded border border-solid border-black shadow font-light py-2 px-3 w-full;
	}

	input[name="title"] {
		@apply text-base;
	}

	input[type="text"]:active,
	input[type="text"]:focus,
	input[type="email"]:active,
	input[type="email"]:focus {
		@apply outline-none;
		border-color: #33A6BA;
	}

	input[type="submit"] {
		background: #33A6BA;
	}
	input[type="submit"]:disabled {
		@apply bg-gray-400;
	}

	input.invalid,
	input.invalid:active,
	input.invalid:focus {
		color: #FF7B92;
		border-color: #FF7B92;
	}

	small::before {
		content:"";
		width:16px;
		height:16px;
		position:absolute;
		bottom:36px;
		background-image: url('../assets/img/error.png');
		right:12px;
		will-change: auto;
	}

	small {
		@apply text-xs;
		color: #FF7B92;
	}

</style>
