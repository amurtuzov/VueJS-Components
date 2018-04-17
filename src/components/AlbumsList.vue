<template>
	<div>
	<transition name="slide-fade">
		<table cellspacing="0" border="1" class="photographer-albums" v-show="photographer.expanded">
			<tbody>
				<tr>
					<td>Album number</td><td><input class="search-field" type="text" v-model="filter" placeholder="Search by album name"></td>
				</tr>
				<tr v-for="album, key in filteredList(photographer.albums)">
					<td>{{++key}}</td><td class="album-name" @click="getPhotos(album.id, album.title)">{{ album.title }}</td>
				</tr>
			</tbody>
		</table>
	</transition>
			<PhotosModal
    		v-if="showModal" 
    		:lazyLoadPhotos="lazyLoadPhotos"
    		:photos="photos"
    		:albumName="albumName"
    		@close="showModal = false" 		
    		/>
	</div>
</template>

<script>
import PhotosModal from './PhotosModal.vue'
export default {
	data () {
		return {
			filter: '',
			photos: [],
			lazyLoadPhotos: [],
			showModal: false,
			albumName: ''
		}
	},
	components: {
		PhotosModal
	},
	props: {
		photographer: {
			type: Object
		}
	},
	methods: {
		getData: function(url){
			return this.axios.get(url)
		},
		filteredList: function(array) {
			if(array != null) {
				let filter = this.filter;
				return array.filter(function (elem) {
					if(filter === '') return true;
					else return elem.title.indexOf(filter) > -1;
				})
			}
		},
		getPhotos: function(id, title) {
			let url ="https://jsonplaceholder.typicode.com/photos/?albumId=" + id;
			this.getData(url).then(function(response) {
				this.photos = response.data;
					this.photos.forEach((photo) => {
						if(this.lazyLoadPhotos.length != 10) {
							this.lazyLoadPhotos.push(photo)
						}
					});
					this.albumName = title;
					this.showModal = true;

			}.bind(this));
		},
	}
}
</script>

<style>
.photographer-albums {
	border-spacing: 0;
	margin-top: 10px;
	margin-left: 10px;
	margin-bottom: 10px;
	width: auto;
	white-space: nowrap;
	background-color: #ffffff;
}

.photographer-albums td {
	padding: 5px;
}

.search-field {
	border: none;
	outline: none;
	width: 100%;
	box-sizing: border-box;
	height: 100%;
}
.album-name {
	cursor: pointer;
	font-style: italic;
	color: #000000;
}
.album-name:first-letter {
	text-transform: uppercase;
}

.slide-fade-enter-active {
  transition: all .3s ease;
}
.slide-fade-leave-active {
  transition: all .5s cubic-bezier(1.0, 0.5, 0.8, 1.0);
}
.slide-fade-enter, .slide-fade-leave-to
/* .slide-fade-leave-active до версии 2.1.8 */ {
  transform: translateY(-50px);
  opacity: 0;
}
</style>