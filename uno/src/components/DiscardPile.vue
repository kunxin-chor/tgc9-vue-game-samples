<template>
    <div id="discard">
        <Card v-if="cards.peek()"
              v-bind:card="cards.peek()"/>
        <div v-else>
            EMPTY!!
        </div>
    </div>
</template>

<script>
import Card from "./Card"
export default {
    component: {
        Card
    },
    props:['cards']    
}
</script>

<style scoped>
    #discard{
        width: 200px;
        height: 150px;
        border:2px red dashed;
    }
</style>