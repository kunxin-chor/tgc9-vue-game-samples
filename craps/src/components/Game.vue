<template>
    <div>
        <div id="rolls">
            <div class="dice" v-for="(d, index) in dice" :key='index'>
                {{d}}
            </div>
        </div>
        <button>Roll</button>
        <button>Withdraw</button>
    </div>
</template>

<script>
export default {
    data: function() {
        return {
            dice:[],
            money: 100,
            history:[]
        }
    },
    created:function() {
        // React -- constructor
        // happens before the first render
        // loading from axios
        // initalizing data to default values
       
        //  instead of : this.dice[0] = 1;
        this.$set(this.dice, 0, 1);

        // instead of : this.dice[1] = 1;
        this.$set(this.dice, 1, 1);
    },
    mounted:function() {
        // React -- componentDidMount
        // happens after the first render
        // etc.timer

    },
    methods:function() {

    }
}
</script>

<style scoped>
.dice {
    border:1px solid black;
    width: 50px;
    height: 50px;
    text-align:center;
    font-size: 32px;
}

#rolls {
    display:flex;
    justify-content:space-around;
}
</style>