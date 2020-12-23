<template>
      <div class="card" :style="{backgroundColor: card.suit}">
            {{card.value}} 
        </div>
</template>

<script>
export default {
    props:['card']
}
</script>

<style scoped>

</style>