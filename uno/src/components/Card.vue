<template>
      <div class="card" :style="{backgroundColor: card.suit,
                                cursor: canDiscard ? 'pointer' : 'default' }"
                        @click='discard'
                                >
            {{card.value}} 
        </div>
</template>

<script>
export default {
    name:'Card',
    props:['card', 'canDiscard'],
    methods:{
        discard:function(){
            if (this.canDiscard) {
                  this.$emit('discardCardEvent', this.card);
            } else {
                alert("This card doesn't match the top card of the pile");
            }
          
        }
    }
}
</script>

<style scoped>
.card {
  border: 1px black solid;
  height: 110px;
  width: 60px;
  border-radius: 10px;
  text-align: center;
  font-size: 24px;
}
</style>