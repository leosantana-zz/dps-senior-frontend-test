<template>
	<div class="card new inline-grid rounded-lg shadow-lg mb-6">
		<transition name="fade" appear>
			<officeCardForm
				:cardColors="cardColors"
				:class="{'open' : showCardForm}"
				v-show="showCardForm" />
		</transition>

		<transition name="fade">
			<div class="front rounded-lg shadow-lg card-red" :class="{'open' : showCardNew}" v-show="showCardNew" >
				<a href="#" @click.prevent="showCardNew = false; showCardForm = true;" class="flex justify-between px-6 py-4 font-hairline text-gray-200 outline-none">
					Add New Location
					<img src="@/assets/img/add.png" class="self-center" />
				</a>
			</div>
		</transition>
	</div>
</template>

<script>
import officeCardForm from "~/components/officeCardForm"

export default {
	components: {
		officeCardForm
	},
	props:{
		cardColors: {
			type: Object,
	 		default: null
		}
	},
	data: () => {
		return {
			showCardForm: false,
			showCardNew: true
		}
	},
	created() {
		this.$nuxt.$on('closeForm',() =>{
			this.showCardForm = false;
			this.showCardNew = true;
			$nuxt.$emit('refresh');
		});
	}
};
</script>

<style scoped>
.fade-enter-active, .fade-leave-active {
  transition: opacity .5s;
}
.fade-enter, .fade-leave-to /* .fade-leave-active em versões anteriores a 2.1.8 */ {
  opacity: 0;
}
.fade-leave-active {
  position: absolute;
  width: 100%;
}


/*
.form {
	transition: all 1s ease;
	z-index:0;
	max-height: 0;
	overflow: hidden;
	opacity:0;
	margin-top: -50px;
}

.form.open {
	margin-top: 0px;
	opacity:1;
	position:relative;
	padding:auto;
	z-index:40;
	max-height: 1000px;
}

.form.open ~ .front {

}

.front {
	transition: opacity 1s ease;
	opacity:0;

}

.front.open {
	opacity:1;

} */
</style>

