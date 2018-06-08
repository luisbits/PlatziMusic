<template lang="pug">
	#app
		img(src='./assets/logo.png')
		h1 PlatziMusic
		select(v-model="selectedCountry")
			option(v-for="country in countries" :value="country.value") {{ country.name }}
		spinner(v-show="loading")
		ul
			artist(v-for="artist in artists" :artist="artist" :key="artist.mbid")
</template>

<script>
import Artist from './components/Artist.vue'
import Spinner from './components/Spinner.vue'
import getArtists from './api'
export default {
  name: 'app',
  data () {
    return {
      artists: [],
			countries: [
				{name: 'Argentina', value: 'argentina'},
				{name: 'Colombia', value: 'Colombia'},
				{name: 'España', value: 'spain'},
				{name: 'México', value: 'mexico'},
			],
			selectedCountry: 'argentina',
			loading: true
    }
  },
  components: {
	  Artist: Artist,
		Spinner: Spinner
  },
	methods: {
		refreshArtists(){
			const self = this
			this.loading = true
			getArtists(this.selectedCountry)
	  		.then(function(artists){
			  self.artists = artists
				self.loading = false
			})
		}
	},
  mounted: function () {
		this.refreshArtists()
  },
	watch: {
		selectedCountry() {
			this.refreshArtists()
		}
	}
}
</script>

<style lang="stylus">
	#app
		font-family 'Avenir', Helvetica, Arial, sans-serif
		-webkit-font-smoothing antialiased
		-moz-osx-font-smoothing grayscale
		text-align center
		color #2c3e50
		margin-top 60px

	h1, h2
		font-weight normal
		color red !important

	ul
		list-style-type none
		padding 0

	li
		display inline-block
		margin 0 10px

	a
		color #42b983
</style>
