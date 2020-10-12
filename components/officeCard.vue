<template>
	<div class="card inline-grid mb-6 overflow-hidden shadow-lg">

		<div class="front relative rounded-lg shadow-lg z-10" :class="[{'open' : showCardFront}, 'card-' + cardData.color]" :key="'cardFront'">
			<a href="#" class="flex justify-between outline-none" @click.prevent="showCardInfo = !showCardInfo;">
				<div>
					<h4 class="font-normal text-white text-xl mb-1">{{ cardData.title }}</h4>
					<p class="font-hairline text-white">{{ cardData.address }}</p>
				</div>
				<img src="@/assets/img/chevron.png" class="self-center" :class="{'arrow-up': showCardInfo}" />
			</a>
		</div>

		<officeCardForm
			:cardData="cardData"
			:cardColors="cardColors"
			:class="{'open' : showCardForm}"
			:key="'cardForm'" />

		<div class="info bg-white rounded-lg shadow-lg px-8 pt-8 pb-6" :class="{'open' : showCardInfo}" >
			<h3 class="text-darkblue font text-xl">{{ cardData.fullName }}</h3>
			<p class="text-darkblue font-light my-2">{{ cardData.jobPosition }}</p>
			<a href="#" class="font-light text-blue">{{ cardData.email }}</a>
			<p class="text-darkblue font-light my-2">{{ cardData.phone }}</p>
			<div class="flex justify-between border border-l-0 border-r-0 border-b-0 pt-4 mt-4">
				<a href="#" @click.prevent="showCardInfo= false; showCardFront= false; showCardForm = true;" class="flex text-gray-500 text-xs font-light outline-none"><img src="@/assets/img/edit.png" class="mr-3" /> EDIT</a>
				<a href="#" @click.prevent="showCardInfo= false; $nuxt.$emit('deleteCard', cardData.id);" class="flex text-red text-xs font-light outline-none"><img src="@/assets/img/delete.png" class="mr-3" /> DELETE</a>
			</div>
		</div>


	</div>
</template>

<script>
import officeCardForm from "~/components/officeCardForm"
export default {
	components: {
		officeCardForm
	},
	data: function() {
		return {
			showCardFront: true,
			showCardInfo: false,
			showCardForm: false,
		}
	},
	props: {
		cardData: {
			type: Object,
	 		default: null
	 	},
		cardColors: {
			type: Object,
	 		default: null
	 	}
	},
	created(){
		this.$nuxt.$on('closeForm', () => {
			this.showCardForm = false;
			this.showCardFront = true;
			$nuxt.$emit('refresh');
		});
	}
};
</script>

<style scoped>
* {
	transform: translateZ(0);
}

.card:not(.new) .front a {
	padding: 3.2rem 1.5rem;
}

.info {
	margin-top: -10px;
}

.front {
	width: 100%;
}
.front img {
	transition: all 300ms ease-in-out;
	transform: rotate(0deg);
}

.front img.arrow-up {
	transform: rotate(180deg);
}

.text-red {color: #FF7B92 }
.text-darkblue { color: #313E4F }
.text-blue { color: #33A6BA }

.info {
	transition: all 750ms ease;
	opacity: 0;
	z-index: 1;
	margin-top:-170px;
	max-height: 0;
	position: relative;
	pointer-events: none;
}

.info.open{
	opacity:1;
	margin-top:-10px;
	pointer-events: all;
	max-height: 1000px;
}

.form {
	transition: all 1s ease;
	z-index:0;
	max-height: 0;
	overflow: hidden;
	opacity:0;
	margin-top: -50px;
}

.form.open {
	margin-top: -160px;
	opacity:1;
	position:relative;
	padding:auto;
	z-index:40;
	max-height: 1000px;
}

.front {
	transition: opacity 1s ease;
	opacity:0;
}

.front.open {
	margin-top:0;
	opacity:1;
}


</style>
