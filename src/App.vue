
<template>
    <HeaderBar :user="user"></HeaderBar>
    <MainView :api="spotifyApi" :refresh_token="refresh_token"></MainView>
    <FooterBar></FooterBar>
</template>

<script>
import HeaderBar from "./components/HeaderBar.vue"
import MainView from "./components/MainView.vue"
import FooterBar from "./components/FooterBar.vue"

import SpotifyWebApi from './spotify-web-api.js'
export default {
    data() {
        return {
            token: "",
            user: "User Info Placeholder",
            spotifyApi : new SpotifyWebApi(),
            refresh_token: ""
        }
    },
    methods: {

    },
    created () {
        var hash = window.location.hash.substr(3);
        var result = hash.split('&').reduce(function (res, item) {
            var parts = item.split('=');
            res[parts[0]] = parts[1];
            return res;
        }, {});
        
        console.log(result)
        console.log(window.location.hash.substr(3))
        let queryToken = result["access_token"]
        if(queryToken == null) {
            window.location.href = "http://superior-shuffle.herokuapp.com/spotify_login"
        }
        this.token = queryToken
        this.refresh_token = result["refresh_token"]
        this.spotifyApi.setAccessToken(queryToken)

    },
    name: "shuffleApp",
    components: {
        HeaderBar,
        MainView,
        FooterBar
    }

}

</script>

<style>

</style>
