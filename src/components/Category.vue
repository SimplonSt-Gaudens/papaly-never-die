<template>
		<v-flex sm4>			
			<v-toolbar>
				<v-toolbar-title>{{category.name}}</v-toolbar-title>
				<v-spacer></v-spacer>
				<v-toolbar-items>
					<v-btn icon @click="display_form">
						<v-icon>
							fas fa-plus
						</v-icon>
					</v-btn>
					<v-btn icon @click="del_cat">
						<v-icon>fas fa-trash</v-icon>
					</v-btn>
				</v-toolbar-items>
			</v-toolbar>
			<v-toolbar color="white" v-if="form_link_status">
				<v-form @submit="add_link">
					<v-text-field 
						type="url" 
						v-model="new_link_url"
						label="add new link"
					>
					</v-text-field>
				</v-form>
			</v-toolbar>
			<Link
				v-for="link in links"
				:link="link"
				:key="link.id"
				v-if="category.id === link.category_id"
				@delete="delete_link"
			/>
		</v-flex>
</template>

<script>
	import Link from './Link'
	import { v4 } from 'uuid'
	import faker from 'faker'

	export default {
		name: "Category",
		props:{
			category: Object
		},
		components:{
			Link
		},
		data(){
			return{
				links:[],
				new_link_name:"",
				new_link_url:"",
				form_link_status: false
			}
		},
		created(){
			for (var i = 0; i < 5; i++) {
				this.links.push(
					{
						id: v4(),
						name: faker.internet.domainName(),
						url: faker.internet.url(),
						category_id: this.category.id
					}
				)
			}
		},
		methods: {
			del_cat(){
				return this.$emit('delete', this.category.id)
			},
			add_link(){
				this.new_link_name = this.new_link_url.split(".")[1]
				this.links.push(
					{
						id:v4(),
						name: this.new_link_name,
						url : this.new_link_url,
						category_id: this.category.id
					}
				)
				this.new_link_url = ""
				this.new_link_name = ""
			},
			delete_link(id){
				this.links = this.links.filter(link => link.id !== id)
			},
			display_form(){
				!this.form_link_status ? this.form_link_status = true : this.form_link_status = false
			}
		}
	}
</script>