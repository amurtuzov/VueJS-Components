<template>
	<li class="photographer">
		<button class="photographer__name" v-on:click="photographer.expanded=!photographer.expanded" :href="photographer.id">{{ photographer.name }}</button>
    <AlbumsList
    :photographer="photographer"
    />
	</li>
</template>

<script>
import AlbumsList from './AlbumsList.vue'
export default {
  components: {
    AlbumsList
  },
  props: {
    photographer: {
      type: Object,
    }
  }
}
</script>


<style>
	.photographer__name {
  background-color: transparent;
  border: none;
  padding: 7px;
  cursor: pointer;
  font-style: italic;
  font-size: 16px;
}
</style>