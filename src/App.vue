
<template>
    <HeaderBar :user="user"></HeaderBar>
    <MainView v-if=" initToken != ''" :refresh_token="refresh_token" :token="token"></MainView>
    <FooterBar></FooterBar>
</template>

<script>
import HeaderBar from "./components/HeaderBar.vue"
import MainView from "./components/MainView.vue"
import FooterBar from "./components/FooterBar.vue"

export default {
    data() {
        return {
            token: "",
            user: "User Info Placeholder",
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
