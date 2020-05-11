<template>
 <v-app light>
   <!--The SideMenu Component goes here-->
  <SideMenu :drawer="drawer"  :api_key="api_key" @selectsource="setResource" ></SideMenu>
   <v-toolbar fixed app light clipped-left color="primary" class="elevation-2">
    <v-toolbar-side-icon @click="drawer = !drawer"  class="white--text"></v-toolbar-side-icon>
    <v-toolbar-title class="white--text"><img src="./assets/images/itunes.png" width="80px" /></v-toolbar-title>
  </v-toolbar>
  <v-content>
    <v-container fluid>

      <!--The MainContent Component goes here-->
      <MainContent :articles="articles"></MainContent>

    </v-container>
   </v-content>
   <v-footer class="secondary" app>
      <v-layout row wrap align-center>
        <v-flex xs12>
          <div class="white--text ml-3">
            Made with
            <v-icon class="red--text">favorite</v-icon>
            by <a class="white--text" href="https://vuetifyjs.com" target="_blank">Vuetify</a>
            and <a class="white--text" href="https://github.com/rachidsakara" target="_blank">Paul Anthony McGowan</a>
          </div>
        </v-flex>
      </v-layout>
    </v-footer>
  </v-app>


</template>

<script>

import axios from 'axios' //importing the axios a HTTP library to connect the app with the API
import SideMenu from './components/SideMenu.vue' // import the SideMenu component
import MainContent from './components/MainContent.vue' // import the MainContent component


export default {

  components: {
      SideMenu,
      MainContent
  },

  data() {
    return {
      drawer: true, // true to show/hide the side navigation drawer
      api_key:'2199a2f9005e4236b0c958097db70f55',
      articles: [],
      errors: []
    }
  },


  created () {
    axios.get('https://newsapi.org/v2/top-headlines?sources=techcrunch&apiKey='+this.api_key)
      .then(response => {
        this.articles = response.data.articles
        console.log('data:')
        console.log(response.data.articles)
      .catch(e => {
        this.errors.push(e)
      })
    })
  },


  methods: {

      setResource(source){

        axios.get('https://newsapi.org/v2/top-headlines?sources='+source+'&apiKey='+this.api_key)
        .then(response => {
          this.articles = response.data.articles
          console.log('Source Articles:')
          console.log(response.data.articles) // This will give you access to the full object
        })
        .catch(e => {
          this.errors.push(e)
        })

      }


   }

  }
</script>
<style scoped>
#app > div > nav > div > div > img {
margin-top: 6px;
}
.secondary {
  background: rgb(3, 24, 56)!important;
}
</style>
