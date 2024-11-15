<template>
	<div class="container">
		<div class="control">
			<p class="title">Параметры</p>
			<div class="recipe-only">
				<input type="checkbox"><p>Показывать только реагенты с рецептами</p>
			</div>
		</div>
		<div class="break"></div>
		<div v-for="(each, index) in a" :key="index" class="item" :id="sanitizeId(each.name)">
			<div class="title">{{ each.name }}</div>
			<div class="desc">{{ each.desc }}</div>
			<div class="recipe-tab">
				<div v-if="each.recipes.length > 0">
					<div v-for="(recipe, recipeIndex) in each.recipes" :key="recipe.typepath" class="recipe">
						<div v-for="(amount, reagentPath, reagentIndex) in recipe.required" :key="reagentPath"
							class="required-item">
							<p class="ingridient">{{ amount }} <a href="#" @click.prevent="scrollToReagent(sanitizeId(getReagentName(reagentPath)))">{{ getReagentName(reagentPath) }}</a></p>
							<span class="add" v-if="reagentIndex < Object.keys(recipe.required).length - 1"> + </span>
						</div>
						<span class="add-end"> = </span>
						<p class="ingridient">{{ recipe.produced_amount }}u</p>
						<span class="add-end" v-if="recipe.required_temp != 0.0">and heat up to {{ recipe.required_temp }} </span>
					</div>
				</div>
				<div v-else>
					Нет доступных рецептов.
				</div>
			</div>
		</div>
	</div>
</template>

<script>
import readjson from '@/scripts/readjson.js'

export default {
	name: 'HomeView',
	data() {
		return {
			a: readjson.data.reagents
		}
	},
	methods: {
		getReagentName(datumPath) {
			const reagent = this.a[datumPath];
			return reagent ? reagent.name : 'Unknown Reagent';
		},
		sanitizeId(name) {
			return name.replace(/\s+/g, '-').replace(/[^\w-]+/g, '');
		},
		scrollToReagent(id) {
			const element = document.getElementById(id);
			if (element) {
				element.scrollIntoView({ behavior: 'smooth', block: 'center' });
				element.classList.add('flash');
				setTimeout(() => {
					element.classList.remove('flash');
				}, 2000);
			}
		}
	},
	components: {}
}
</script>

<style lang="css" scoped>
@media (max-width: 999px) {
    .item {
	width: calc(100% - 23px) !important;
   }
    .container {
	width: calc(100vw - 0px) !important;
	margin-left: 0px !important;
	padding-right: 10px !important;
   }
    .control {
	width: calc(100vw - 23px) !important;
   }
}


.break {
	height: 20px;
}

input {
	margin-right: 10px;
}

.recipe-only {
	display: flex;
	margin-top: -20px;
}

.item {
	border: 2px solid black;
	width: 70%;
	padding: 10px;
	margin-right: 10px;
}

.control {
	border: 2px solid black;
	width: 70%;
	padding: 10px;
	margin-top: 50px;
}

.title {
	margin-top: -20px;
	background-color: white;
	width: fit-content;
	padding-left: 10px;
	padding-right: 10px;
}

.container {
	display: flex;
	flex-direction: column;
	gap: 15px;
	margin-left: 20vw;
	padding-right: 10px;
}

.recipe-tab {
	width: 100%;
}

.recipe {
	display: flex;
	flex-wrap: wrap;
	gap: 10px;
	width: 100%;
}

.required-item {
	width: fit-content;
	display: flex;
	flex-direction: row;
}

.ingridient {
	position: relative;
	border: 1px solid black;
	padding: 2px;
	padding-right: 5px;
	padding-left: 5px;
	height: fit-content;
	width: fit-content;
	margin-top: 5px;
	margin-bottom: 0px;
}

.add {
	position: relative;
	margin-top: 8px;
	margin-left: 10px;
}

.add-end {
	margin-left: 5px;
	margin-right: 5px;
	position: relative;
	margin-top: 8px;
}

.flash {
	animation: flash-animation 2s forwards;
}

@keyframes flash-animation {
	0% {
		background-color: rgb(109, 93, 116);
	}
	100% {
		background-color: transparent;
	}
}
</style>
