<template>
    <div id="header">
        <p style="text-align: right;">{{ user }}</p>
    </div>
    <main>

    </main>
    <footer>

    </footer>
</template>

<script>
import SpotifyWebApi from './spotify-web-api.js'
export default {
    data() {
        return {
            token: "",
            user: "User Info Placeholder"
        }
    },
    methods: {

    },
    name: "shuffleApp"

}
var spotifyApi = new SpotifyWebApi()

fetch("https://accounts.spotify.com/authorize", {
    body : {
        response_type: "code",
        client_id: "5f0d2fec492d4f35a726b74b099bd420",
        // web API only needs user playlist info, playback control is done through playback SDK
        scope: " playlist-read-private playlist-read-collaborative",
        redirect_uri: "http://localhost:8080"

    }
})


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
