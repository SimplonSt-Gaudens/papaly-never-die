<template>
	<section>
		<v-toolbar>
			<v-toolbar-title>{{board.name}}</v-toolbar-title>
			<v-spacer></v-spacer>
			<v-toolbar-items>
				<v-btn icon @click="display_form">
					<v-icon>
						fas fa-plus
					</v-icon>
				</v-btn>
			</v-toolbar-items>
		</v-toolbar>
		<v-toolbar color="white" v-if="form_cat_status">
			<v-form @submit="add_cat">
				<v-text-field 
					type="text" 
					v-model="new_category"
					label="add new category"
				>
				</v-text-field>
			</v-form>
		</v-toolbar>
		<v-layout wrap>
			<Category 
				v-for="category in categories" 
				:category="category" 
				:key="category.id"
				@delete="delete_cat"
			/>
		</v-layout>
	</section>
</template>

<script>
	import Category from "./Category"
	import { v4 } from 'uuid'
	import faker from 'faker'

	export default {
		name : "Board",
		props : {
			board: Object
		},
		components:{
			Category
		},
		data(){
			return{
				categories:[],
				new_category: "",
				form_cat_status: false
			}
		},
		created(){
			for (var i = 0; i < 5; i++) {
				this.categories.push(
					{
						id: v4(),
						name: faker.random.word(),
						board_id: this.board.id
					}
				)
			}
		},
		methods: {
			add_cat(e){
				e.preventDefault()
				this.categories.push(
					{
						id:v4(),
						name: this.new_category,
						board_id: this.board.id
					}
				)
				this.new_category = ""
			},
			delete_cat(id){
				this.categories = this.categories.filter(category => category.id !== id)
			},
			display_form(){
				!this.form_cat_status ? this.form_cat_status = true : this.form_cat_status = false
			}
		}
	}
</script>