<template>
	<v-app>
		<v-layout>
			<v-navigation-drawer permanent>
				<v-toolbar flat>
					<v-list>
						<v-list-tile>
							<v-list-tile-title class="title">
								Papaly Never Die
							</v-list-tile-title>
							<v-btn icon @click="display_form">
								<v-icon>fas fa-plus</v-icon>
							</v-btn>
						</v-list-tile>
					</v-list>
				</v-toolbar>
				<v-toolbar v-if="form_board_status">
					<v-form @submit="add_board">
						<v-text-field 
							type="text" 
							v-model="new_board"
							label="new board"
						>
						</v-text-field>
					</v-form>
				</v-toolbar>
				<v-divider></v-divider>
				<v-list>
					<v-list-tile
						v-for="board in boards"
						:key="board.id"
						@click="select_board(board.id)"
					>
						{{ board.name }}
						<v-spacer></v-spacer>
						<v-btn @click="delete_board(board.id)" icon>
							<v-icon>fas fa-trash</v-icon>
						</v-btn>
					</v-list-tile>
				</v-list>
			</v-navigation-drawer>
			<v-flex sm10>
				<Board
					v-for="board in boards"
					v-if="board_selected === board.id" 
					:board="board" 
					:key="board.id"
				/>
			</v-flex>
		</v-layout>
	</v-app>
</template>

<script>
	import Board from '@/components/Board'
	import { v4 } from 'uuid'
	import faker from 'faker'

	export default {
		name: "Papaly",
		components:{
			Board
		},
		data(){
			return {
				boards:[],
				board_selected: null,
				new_board: "",
				form_board_status: false
			}
		},
		created(){
			for (var i = 0; i < 5; i++) {
				this.boards.push(
					{
						id : v4(),
						name: faker.random.word()
					}
				)
			}
			this.board_selected = this.boards[0].id
		},
		methods: {
			select_board(id){
				this.board_selected = id;
			},
			add_board(e){
				e.preventDefault
				this.boards.push({
					id:v4(),
					name: this.new_board
				})
				this.new_board = ""
			},
			delete_board(id){
				this.boards = this.boards.filter(board => board.id !== id)
			},
			display_form(){
				!this.form_board_status ? this.form_board_status = true : this.form_board_status = false
			}
		}
	}
</script>