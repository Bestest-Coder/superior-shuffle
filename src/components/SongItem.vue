<template>
    <draggable
    v-model="{songArray}"
    group="songs"
    @start="drag=true"
    @end="drag=false"
    item-key="id">
    <template #item="{song}">
        <div>{{song.title}}</div>
    <template>
</template>

<script>
import draggable from 'vuedraggable'

export default {
    components: {
        draggable,
    },
    data() {
        return {
            drag: false.
        }
    },
    name: "songArrayDisplay",
    props: {
        songArray
    }
}