<template>
  <div id="app">
    <img src="dist/logo.png">
    <h1> PlatziMusic </h1>
    <select v-model="selectedCountry">
      <option v-for="country in countries" v-bind:value="country.value"> {{country.name}} </option>
    </select>
    <spinner v-show="loading"></spinner>
    <ul>
      <!-- <li v-bind:key="artist" v-for="artist in artists" >{{ artist.name }}</li> -->
      <artist v-for="artist in artists" 
      v-bind:key="artist.mbid" v-bind:artist="artist"> </artist>
    </ul>
  </div>
</template>

<script>
import Artist from './components/Artist.vue'
import getArtists from './api'
import Spinner from './components/Spinner.vue'

export default {
  name: 'app',
  data () {
    return {
      artists: [],
      countries:[
        { name: 'Colombia', value: 'colombia' },
        { name: 'Argentina', value: 'argentina' },
        { name: 'España', value: 'spain' }
      ],
      selectedCountry: 'argentina',
      loading: true
    }
  },
  components: {
    Artist: Artist,
    Spinner
  },
  methods:{
    refreshArtist(){
      const self = this;
      this.loading = true;
      this.artists = [];
      getArtists(this.selectedCountry)
      .then( function (artists){
      self.artists = artists;
      self.loading = false;
      });
    }
  },
  mounted(){
    this.refreshArtist();
  },
  watch: {
    selectedCountry(){
      this.refreshArtist();
    }
  }
}
</script>

<style>
#app {
  font-family: 'Avenir', Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}

h1, h2 {
  font-weight: normal;
}

ul {
  list-style-type: none;
  padding: 0;
}

li {
  display: inline-block;
  margin: 0 10px;
}

a {
  color: #42b983;
}
</style>
