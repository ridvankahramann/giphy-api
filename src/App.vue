<template>
  <div id="app">
    <search v-on:SearchRequested="handleSearch"></search>
    <p v-if="isloading">Loading</p>
    <preview :gifs=gifs></preview>
    <p v-if="isdata">Böyle bir gift yok...</p>
  </div>
</template>

<script>
import Search from './compoments/Search.vue'
import Preview from './compoments/Preview.vue'

export default {
  name: 'app',
  components: { Search, Preview },
  data () {
    return {
      isloading: true,
      isdata: false,
      gifs: []
    }
  },
  methods: {
    doQuery(url){
      fetch(url)
      .then(res => res.json())
      .then(res => {
        if(res.data.length == 0){
          this.isdata = true;
        }else{
          this.gifs = res.data;
          this.isloading = false;
          this.isdata = false;
        }
        })  
    },
    handleSearch(query){
      this.gifs = [];
      const url = `http://api.giphy.com/v1/gifs/search?q=${query}&api_key=dc6zaTOxFJmzC`
      this.doQuery(url);
    }
  },
  created(){
    const url = 'http://api.giphy.com/v1/gifs/trending?api_key=dc6zaTOxFJmzC';
    this.doQuery(url);
  }
}
</script>

<style lang="scss">
#app {
  font-family: 'Avenir', Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
}
</style>
