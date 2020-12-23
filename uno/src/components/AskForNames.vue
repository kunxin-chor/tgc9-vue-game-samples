<template>
<div>
    <h1>Welcome to UNO</h1>
    <div>
        <label>Player 1</label>
        <input type="text" v-model="player1Name"/>
    </div>
    <div>
        <label>Player 2</label>
        <input type="text" v-model="player2Name"/>
    </div>
    <button @click="begin">Begin game</button>
</div>
</template>

<script>
export default {
    data: function() {
        return {
            player1Name:"",
            player2Name:""
        }
    },
    methods:{
        begin:function(){
            this.$emit('beginGameEvent', this.player1Name, this.player2Name)
        }
    }
}
</script>

<style scoped>

</style>