<script setup>
import { onMounted, ref } from 'vue';
const props = defineProps({
    number:Number,
    tileid : Number
})

function dragStart(e){
    e.dataTransfer.effectAllowed = 'move'
    e.dataTransfer.dropEffect = 'move'
    e.dataTransfer.setData("number", props.number)
    e.dataTransfer.setData("id", props.tileid)
}


const Tile = ref(null)
onMounted(()=>{
    Tile.value.dataset.id = props.tileid

})




</script>

<template>

<div class="tile" draggable="true" @dragstart="dragStart" ref="Tile">
    {{ props.number }}
</div>

</template>
<style scoped>
.tile{
    color: white;
    background-color: #2563eb;
    display: flex;
    align-items: center;
    justify-content: center;
    font-size: 56px;
    font-family: regular-bold;
    cursor: grab;
    width: 100%;
    height: 100%;
}
</style>