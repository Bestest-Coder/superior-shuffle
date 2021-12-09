<template>
    <HeaderBar :user="{{user}}"></HeaderBar>
    <MainView></MainView>
    <FooterBar></FooterBar>
</template>

<script>
import HeaderBar from './components/HeaderBar.vue'
import MainView from './components/MainView.vue'
import FooterBar from './components/FooterBar.vue'

import SpotifyWebApi from './spotify-web-api.js'
export default {
    data() {
        return {
            token: "",
            user: "User Info Placeholder",
            spotifyApi : new SpotifyWebApi()
        }
    },
    methods: {

    },
    created () {
        let params = new URLSearchParams(document.location.search)
        let queryToken = params.get("access_token")
        if(queryToken == null) {
            window.location.href = "superior-shuffle.herokuapp.com/spotify_login"
        }
        this.token = queryToken
        this.spotifyApi.setAccessToken(queryToken)

    },
    name: "shuffleApp"

}

</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>
