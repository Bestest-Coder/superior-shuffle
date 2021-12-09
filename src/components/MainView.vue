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
    </div>
</template>

<script>
import draggable from "vuedraggable"
export default {
    name: "MainView",
    components: {
        draggable
    },
    data() {
        return {
            playingList: [{name: "Hooked on a Feeling", artist: "Blue Sweede"}],
            userPlaylists: [],
            searchList: []
        };
    },
    methods: {

    },
    created() {

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