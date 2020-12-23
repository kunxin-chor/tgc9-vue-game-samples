<template>
      <div class="card" :style="{backgroundColor: card.suit}">
            {{card.value}} 
        </div>
</template>

<script>
export default {
    name:'Card',
    props:['card']
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