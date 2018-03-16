<template>
	<ul class="photographers">
		<PhotographersListItem
		v-for="photographer in photographers" :key="photographer.id" :photographer="photographer"
		/>
	</ul>
</template>

<script>
	import PhotographersListItem from './PhotographersListItem.vue'

	export default {
		components: {
			PhotographersListItem
		},
  		data () {
    		return {
    			photographers: []
    		}
		},
  		mounted() {
  			// this.notLoaded();
			Promise.all([this.axios.get('https://jsonplaceholder.typicode.com/users'), this.axios.get('https://jsonplaceholder.typicode.com/albums')])
			.then(function(response){
				this.photographers = response[0].data;
				this.photographers.forEach((photographer) => {
					let albums = []
					this.$set(photographer, 'expanded', false);
					response[1].data.forEach((album) => {
						if(photographer.id == album.userId) {
							albums.push(album)
							this.$set(photographer, 'albums', albums);
						}
					})
				})
				this.loaded();
			}.bind(this));
  		},
  		methods: {
  			notLoaded() {
  				this.$emit('notLoaded');
  			},
  			loaded() {
  				this.$emit('loaded');
  				console.log(1)
  			}
  		}
  	}
</script>

<style>
	.photographers {
	padding: 0;
	list-style: none;
	display: flex;
	flex-direction: column;
	justify-content: space-around;
}
</style>