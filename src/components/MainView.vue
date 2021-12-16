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
                        <button @click="logPlaylistSongs(playlist.uri)">Log playlist's songs</button>
                        <button @click="getPlaylistSongs(playlist.uri, playlist)">Get playlists's songs</button>
                            <draggable
                              :list="playlist.songs"
                              group="songs"
                              itemKey="name">
                                <template #item="{ element }">
                                    <SongDisplay :songData="element"></SongDisplay>
                                </template>
                            </draggable>
                    </li>
                </ul>
            </div>
        </div>
    </div>
</template>

<script>
import draggable from "vuedraggable"
import SpotifyWebApi from '../spotify-web-api'
import SongDisplay from './SongDisplay.vue'

export default {
    name: "MainView",
    props: {
        refresh_token: String,
        initToken: String
    },
    components: {
        draggable,
        SongDisplay
    },
    data() {
        return {
            userID: "",
            api: new SpotifyWebApi(),
            token: "",
            playingList: [{name: "fuckle"}, {name: "buckle"}],
            userPlaylists: [],
            searchList: []
        };
    },
    methods: {
        logPlaylists() {
            console.log(this.userPlaylists)
        },
        logPlaylistSongs(playlistURI) {
            console.log(playlistURI)
            console.log(playlistURI.substring(17))
            this.api.getPlaylistTracks(playlistURI.substring(17)).then((value) => {console.log(value)}).catch(this.apiErrorCatch)
        },
        // gets songs from a playlistURI, iterate though them adding to a list, then sets the .songs attribute of playlist to that list
        getPlaylistSongs(playlistURI, playlist) {
            this.api.getPlaylistTracks(playlistURI.substring(17)).then((value) => {
                let outSongList = []
                value.items.forEach(element => {
                    outSongList.push(element.track)
                })
                playlist.songs = outSongList
            }).catch(this.apiErrorCatch)
        },
        logSong(song) {
            console.log(song)
        },
        // callback to use in api promises to catch if token has expired
        apiErrorCatch(error) {
            console.log(error)
            if(error.status === 401){
               fetch("http://superior-shuffle.herokuapp.com/spotify_refresh?refresh_token="+this.refresh_token).then((value) => {
                   console.log(value)
                   this.api.setAccessToken(value.json()["access_token"])
                   this.token = value.json()["access_token"]
               })
            }
        }
    },
    created() {
        //initialize this.api
        this.api.setAccessToken(this.initToken)
        this.token = this.initToken
        
        /* gets the current userID
         * sets that id to this.userID
         * uses that ID to fetch playlists of the user, excluding ones made by spotify
         * appends those playlists to this.userPlaylists
         */
        this.api.getMe().then((value) => {this.userID = value.id; return value.id}).then(
            (value)=> {return this.api.getUserPlaylists(value)}).then(
                (value) => {

                    let outList = []
                    value.items.forEach(element => {
                        if(element.owner.display_name != 'Spotify') {
                            outList.push(element)
                        }
                    })
                    
                    this.userPlaylists = outList
                }).catch(this.apiErrorCatch)
        
    }
}
</script>

<style scoped>
.full-container {
    display: flex;
    flex-direction: row;
}

.playingList-display {
    flex-grow: 1;
}

.inputLists-display {
    flex-grow: 1;
    
    display: flex;
    flex-direction: column;
}

.songSearch-display {
    flex-grow: 1;
}

.userPlaylists-display {
    flex-grow: 1;
}

</style>