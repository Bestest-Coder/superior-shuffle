<template>
    <div class="full-container">
        <div class="playinglist-display">
            <draggable
              :list="playingList"
              group="songs"
              itemKey="name">
                <template #item="{ element }">
                    <div class="song-display"> {{element.name}}</div>
                </template>
            </draggable>
        </div>
        <div class="inputLists-display">
            <!--
            <div class="songSearch-display">
                <draggable
                :list="searchList"
                group="songs"
                itemKey="name">
                    <template #item="{ element }">
                        <div class="song-display"> {{element.name}}</div>
                    </template>
                </draggable>
            </div>
            -->
            <div class="userPlaylists-display">
                <ul>
                    <button @click="logPlaylists">print playlists</button>
                    <li v-for="playlist in userPlaylists" :key="playlist.name">
                        {{playlist.name}}
                    </li>
                </ul>
            </div>
        </div>
    </div>
</template>

<script>
import draggable from "vuedraggable"
import SpotifyWebApi from '../spotify-web-api';
export default {
    name: "MainView",
    props: {
        api: SpotifyWebApi
    },
    components: {
        draggable
    },
    data() {
        return {
            userID: "",
            playingList: [{name: "Hooked on a Feeling", artist: "Blue Sweede"},{name: "Kickstart my Heart", artist: "Motley Crue"}],
            userPlaylists: [{name: "please do the thing"}],
            searchList: []
        };
    },
    methods: {
        logPlaylists() {
            console.log(this.userPlaylists[0])
        }
    },
    created() {
        this.api.getMe().then((value) => {this.userID = value.id; return value.id}).then(
            (value)=> {return this.api.getUserPlaylists(value)}).then(
                (value) => {
                    console.log(value.items)
                    let outList = []
                    value.items.forEach(element => {
                        if(element.owner.display_name != 'Spotify') {
                            outList.push({name: element.name})
                        }
                        console.log(element.name)
                    })
                    console.log(outList)
                    this.userPlaylists = outList
                })
        
    }
}
</script>

<style scoped>
.full-container {
    display: flex;
    flex-direction: row;
}

.song-container {
    display: flex;
    flex-direction: column;
}

</style>