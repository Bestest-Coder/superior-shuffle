<template>
    <div>
        <img :src="songData.album.images[0].url" :alt="songData.album.name" width=100 height=100>
        <p>{{songData.name}}</p>
    </div>
</template>

<script>

export default {
    name: "songDisplay",
    props: {
        songData: Object
    }
}

</script>

<style scoped>

div {
    background: wheat;
    
    outline-style: solid;
    outline-color: rgb(160, 160, 7);
    outline-width: 3px;
    
    margin: 10px;

    border-radius: 15px;
    padding: 0.5%;
    padding-left: 1%;
}

</style>