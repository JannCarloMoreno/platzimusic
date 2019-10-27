<template lang="pug">
  #app
    img(src='https://janncarlomoreno.github.io/platzimusic/dist/logo.png')
    h1 Platzi Music
    select(v-model="selectedCountry")
      option(v-for="country in countries" :value="country.value") {{country.name}}
    spinner(v-show="loading")
    ul
      artist(v-for="artist in artists" :artist="artist" :key="artist.mbid")
</template>

<script>
import getArtist from './API'
import Artist from './components/Artist'
import Spinner from './components/Spinner'
export default {
  
  name: 'app',
  data () {
    return {
      artists: [],
      countries: [
        {name: "Argentina", value:"argentina"},
        {name: "Colombia", value:"colombia"},
        {name: "España", value:"spain"}
      ],
      selectedCountry: "argentina",
      loading: true
    }
  },
  components:{
    Artist,
    Spinner
  },
  methods: {
    refreshArtists(){
      const self = this
      this.loading = true
      this.artists=[]
      getArtist(this.selectedCountry)
        .then( function (artists){
         self.loading = false
          self.artists = artists
      } )
  }
  },
  mounted(){
    this.refreshArtists()
  } ,
  watch: {
    selectedCountry(){
      this.refreshArtists()
    }
  },
}
</script>

<style lang="stylus">
#app 
	font-family 'Avenir', Helvetica, Arial, sans-serif
	-webkit-font-smoothing antialiased
	-moz-osx-font-smoothing grayscale
	text-align center
	color red !important
	margin-top 60px
h1, h2 
	font-weight normal
ul 
	list-style-type none
	padding 0
a 
	color #42b983
</style>
