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
    outline-style: solid;
}

</style>