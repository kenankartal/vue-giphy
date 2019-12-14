<template>
  <div id="app">
    <img src="./assets/logo.png"/>
    <p>Hello, I am a 3rd grade computer engineering student. I work part-time in a company.
      My experience at the company on java and angular.</p>
    <p>
      I do research and exercises to improve myself in my time outside the university and the company.
      This is my first experience in vue. </p>
    <p>see my work in my github account.</p>
    <a href="https://github.com/kenankartal" target="_blank">
      <img src="./assets/github.png" style="height: 40px;"/>
    </a>
    <Search v-on:SearchRequested="handleSearch"></Search>
    <p v-if="isLoading">Loading</p>
    <Preview :gifs=gifs></Preview>
    <router-view/>
  </div>
</template>

<script>
  import Search from "./components/Search";
  import Preview from "./components/Preview";

  export default {
    name: 'app',
    components: {Search, Preview},
    data() {
      return {
        isLoading: true,
        gifs: []
      }
    },
    methods: {
      handleSearch(query) {
        this.gifs = [];
        this.isLoading = true;
        fetch(`http://api.giphy.com/v1/gifs/search?q=${query}&api_key=dc6zaTOxFJmzC`)
          .then((res) => {
            return res.json()
          })
          .then((res) => {
            this.gifs = res.data;
            this.isLoading = false;
          })
      }
    },
    created() {
      fetch('http://api.giphy.com/v1/gifs/trending?api_key=dc6zaTOxFJmzC')
        .then((res) => {
          return res.json()
        })
        .then((res) => {
          this.gifs = res.data;
          this.isLoading = false;
        })
    }
  }
</script>

<style lang="scss">
  #app {
    font-family: 'Avenir', Helvetica, Arial, sans-serif;
    -webkit-font-smoothing: antialiased;
    -moz-osx-font-smoothing: grayscale;
    text-align: center;
    margin-top: 60px;
  }

</style>
