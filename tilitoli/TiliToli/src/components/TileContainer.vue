<script setup>
import Tile from './Tile.vue';
import { TILEIDLIST } from '@/global';
import { createApp, onBeforeMount, onMounted, ref } from 'vue';
let tile = null;
const numbers = [1,2,3,4,5,6,7,8,9,10,11,12,13,14,15]
const NumbersOrdered = [...numbers]

numbers.sort(() => (Math.random() > .5) ? 1 : -1);
let posofEmpty = 4;
const counter = ref(0)
let stepCount = 0;
const tiles = ref(null)

while(true){
    if(CheckForParity(numbers) == false){
        break;
    }
    numbers.sort(() => (Math.random() > .5) ? 1 : -1);

}



function InversionCount(arr){
    let invcount = 0;
    for (let i = 0; i < arr.length; i++) {
        for (let j = i; j < arr.length; j++) {
            if(arr[i] > arr[j]){
                invcount++
            }
        }
    }

    return invcount
}

function CheckForParity(arr){
    if(posofEmpty % 2 == 0){
        if(InversionCount(arr) % 2 == 0){
            return true
        }
            return false;
        
    }else{
        if(InversionCount(arr) % 2 != 0 ){
            return true;
        }
        return false;
    }
}


onMounted(()=>{
    let i = 0
    tiles.value.childNodes.forEach(element => {
        if(element.classList){
            element.dataset.order = i++;

            
        }
    });
})


function onDrop(e){
    const number = e.dataTransfer.getData("number")
    const tileid = e.dataTransfer.getData("id")
    if(e.target.childNodes.length != 0){
        console.log("FOGLALT");
        return
    }
    let currentOrder = []
    let Correct = true;
    const tiles = document.querySelectorAll(".tile");
    let canExecute = true
    tiles.forEach(tile => {
        if(tile.dataset.id == tileid){

            let toOrderId = +e.target.dataset.order
            let parentNodeId = +tile.parentNode.dataset.order
            if(toOrderId != parentNodeId - 1 && toOrderId != parentNodeId + 1 && toOrderId != parentNodeId - 4 &&toOrderId != parentNodeId + 4){
                console.log("NEM MELLETI")
                canExecute = false;
            }

            if(canExecute){
                tile.remove();
                
                let index = TILEIDLIST.indexOf(tileid);
                TILEIDLIST.splice(index,1)
            }
        }
    });
    if(canExecute){
        let randint = getRandomInt(1000)
        tile = createApp(Tile, {number:+number, tileid:randint})
        tile.mount(e.target)
    }

    const tilesForCheckingAfterMove = document.querySelectorAll(".tile");
    tilesForCheckingAfterMove.forEach(tile => {
        currentOrder.push(+tile.innerText)
        
    });

    for (let index = 0; index < currentOrder.length; index++) {
        if(currentOrder[index] != NumbersOrdered[index]) {
            Correct = false;
        }
    }

    if(Correct){
        alert("SIKERES MEGOLDÁS")
    }



}



function getRandomInt(max) {
    let randint = Math.floor(Math.random() * max)

    if(TILEIDLIST.includes(randint)){
        let notUnique = true
        while(notUnique){
            randint = Math.floor(Math.random()*max)
            if(!TILEIDLIST.includes(randint)){
                TILEIDLIST.push(randint)
                notUnique = false;
            }
        }
    }else{
        TILEIDLIST.push(randint)
    }
    
    return randint;
}
</script>
<template>
    <div class="wrapper">
        <div class="tiles" ref="tiles">
            <div class="tile-wrapper" @drop="onDrop" @dragenter.prevent @dragover.prevent ></div>
            <div class="tile-wrapper" v-for="number in numbers" @drop="onDrop" @dragenter.prevent @dragover.prevent >
                <Tile :number="number" :tileid="getRandomInt(1000)"/>
            </div>
        </div>
        <div ref="counter">
        </div>
    </div>

</template>
<style scoped>
    .wrapper{
        width: 70%;
        display: flex;
        justify-content: center;
        align-items: center;
        
    }

    .tiles{
        display: grid;
        width: 50%;
        grid-template-columns: 1fr 1fr 1fr 1fr;
        grid-template-rows: 1fr 1fr 1fr 1fr;
        aspect-ratio: 1/1;
        gap: 15px;
    }
</style>