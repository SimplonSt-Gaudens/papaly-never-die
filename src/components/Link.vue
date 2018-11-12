<template>
	<v-list>
		<v-list-tile>
			<v-list-tile-title>
				<a :href="link.url">{{ link.name }}</a>
			</v-list-tile-title>
			<v-btn @click="del_link" icon>
				<v-icon>fas fa-trash</v-icon>
			</v-btn>
		</v-list-tile>
		<v-divider></v-divider>
	</v-list>
</template>

<script>
	export default {
		name: "Link",
		props: {
			link: Object
		},
		methods: {
			del_link(){
				this.$emit('delete', this.link.id)
			}
		}
	}
</script>