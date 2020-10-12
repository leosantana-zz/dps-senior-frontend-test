<template>
	<div class="home-container relative pt-40 mx-auto">
		<h1 class="block text-center text-6xl font-hairline mb-8">Offices</h1>

		<officeCardNew :cardColors="cardColors" />

		<transition-group name="card" tag="div" mode="out-in" appear>
			<officeCard
				v-for="(officeCard, id) in officeCards"
				:key="officeCard"
				:cardData="{'id':id, ...officeCard}"
				:cardColors="cardColors"
				class="card" />
		</transition-group>

	</div>
</template>

<script>

import officeCard from "~/components/officeCard"
import officeCardNew from "~/components/officeCardNew"

export default {
	components: {
		officeCard,
		officeCardNew
	},
	data() {
		return {
			officeCards: [
				{
					"title" : "Planetbiz",
					"address" : "1557  Pritchard Court",
					"fullName" : "Karol A Gilbert",
					"jobPosition" : "Physical Therapist",
					"email" : "18b0hmrowqyh@temporary-mail.net",
					"phone" : "(507) 319-2012",
					"color" : "darkblue"
				},
				{
					"title" : "Circuit City",
					"address" : "874  Cherry Tree Drive",
					"fullName" : "Rachel J Hamilton",
					"jobPosition" : "Correspondence Clerk",
					"email" : "bvu8mowjmh@temporary-mail.net",
					"phone" : "(904) 356-6515",
					"color" : "red"
				},{
					"title" : "Ideal Garden Maintenance",
					"address" : "3818  Shinn Avenue",
					"fullName" : "Howard R Young",
					"jobPosition" : "Social and Human Service Assistant",
					"email" : "lxdb8xcx0sd@temporary-mail.net",
					"phone" : "(724) 577-9589",
					"color" : "gray"
				},{
					"title" : "Eletronic Source",
					"address" : "4769  Raoul Wallenberg Place",
					"fullName" : "Clarice K Click",
					"jobPosition" : "SInsurance Claim and Policy Processing Clerk",
					"email" : "k2l2g9ui31@temporary-mail.net",
					"phone" : "(203) 816-6824",
					"color" : "blue"
				},{
					"title" : "Dynatronics Accessories",
					"address" : "2666  Whaley Lane",
					"fullName" : "Denise J Dawson",
					"jobPosition" : "Supervisor",
					"email" : "ty1eynqk6tc@temporary-mail.net",
					"phone" : "(262) 424-4178",
					"color" : "darkblue"
				},{
					"title" : "Strategy Planner",
					"address" : "1035  Pleasant Hill Road",
					"fullName" : "James R Gary",
					"jobPosition" : "Agricultural Inspector",
					"email" : "clau_toddyn@msn.com",
					"phone" : "(562) 226-5573",
					"color" : "yellow"
				}
			],
			cardColors: {
				'yellow': '#FFC062',
				'red': '#FF7B92',
				'blue': '#33A6BA',
				'gray': '#989EA7',
				'darkblue': '#313E4F'
			}
		}
	},
	methods:{
		createStyleCardColors() {
			let styleEl = document.createElement('style');
			styleEl.type = 'text/css';
			document.head.appendChild(styleEl);
			let styleClasses = '';

			Object.keys(this.cardColors).forEach((color) => {
				styleClasses += `.card-${color} {
									background-color: ${this.cardColors[color]};
								}
								.${color} {
									color: ${this.cardColors[color]};
								}`;
			});

			styleEl.innerHTML = styleClasses;
		}
	},
	created() {
		this.$nuxt.$on('deleteCard', (id) => {
			this.officeCards.splice(id,1);
			this.$forceUpdate();
		});
		this.$nuxt.$on('submitCard', (form) => {
			if(typeof form.id === 'undefined'){
				this.officeCards.unshift({...form});
			} else {
				this.officeCards[form.id] = {...form};
			}
			this.$forceUpdate();
		});
		this.$nuxt.$on('refresh', () => {
			this.$forceUpdate();
		});
	},
	mounted() {
		this.createStyleCardColors();
	}
};
</script>

<style>
* {
	transform: translateZ(0);
}

.home-container { width: 318px; }
h1 { color: #33A6BA; }

.card {
	transition: all 0.7s;
	width:100%;
}
.card-enter, .card-leave-to
{
	opacity: 0;
}
.card-enter-to {
	opacity: 1;
}

.card-leave-active {
	position: absolute;
	left:0;
}

</style>
