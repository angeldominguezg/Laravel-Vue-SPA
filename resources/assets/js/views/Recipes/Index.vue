<template>
	<div class="recipe__list">
		<div class="recipe__item" v-for="recipe in recipes">
			<router-link class="recipe__inner" :to="`/recipes/${recipe.id}`">
				<img :src="`/images/${recipe.image}`" v-if="recipe.image">
				<p class="recipe__name">{{recipe.name}}</p>
			</router-link>
		</div>
	</div>
</template>

<script>

	import { get } from '../../helpers/api'

	export default {
		data() {
			return {
				recipes: []
			}
		},

		created() {
			get('/api/recipes') // Recipes index
			.then((res) => { 
 				console.log(res.data)
				this.recipes = res.data.recipes

			})

		} 
	}
</script>
