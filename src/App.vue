<template>
  <div class="app-container">
    <top-bar></top-bar>
    <loader v-if="isLoading"></loader>
    <grid :users="users"></grid>
    <fetch-button @buttonClicked="handleClick" v-model="page"></fetch-button>
  </div>
</template>

<script>
  import Loader from './Loader'
  import TopBar from './TopBar'
  import Grid from './Grid'
  import FetchButton from './FetchButton'
  import { Service } from './Service'

  export default {
    name: 'App',
    components: {
      TopBar,
      Loader,
      Grid,
      FetchButton,
    },
    data() {
      return {
        isLoading: true,
        users: [],
        page: 1,
      }
    },
    created() {
      this.fetchData()
    },
    methods: {
      fetchData() {
        return Service.fetch({ page: this.page })
          .then(res => {
            this.isLoading = false
            this.users = this.users.concat(res.results)
          })
      },
      handleClick() {
        this.page++
        this.fetchData()
      },
    },
  }
</script>

<style lang="scss">
  @import "node_modules/normalize-scss/sass/_normalize";
  @include normalize();

  *, *:before, *:after {
    box-sizing: border-box;
  }

  body {
    font-size:62.5%;
    font-family: Helvetica, sans-serif;
    background: #eee;
  }
</style>
