<template>
  <div id="app">
    <PhotographersList
    :showPreloader="showPreloader"
    @loaded="showPreloader = false"/>
    <Preloader
    v-if="showPreloader"
    />
  </div>
</template>

<script>
import PhotographersList from './components/PhotographersList.vue'
import Preloader from './components/Preloader.vue'

export default {
	data() {
		return {
			showPreloader: true
		}
	},
	components: {
		PhotographersList,
		Preloader
	},
}
</script>

<style>
html, body {
  height: 100%;
  padding: 0;
  margin: 0;
}
body {
	padding-top: 1px;
}
#app {
	padding-top: 1px;
}
</style>
