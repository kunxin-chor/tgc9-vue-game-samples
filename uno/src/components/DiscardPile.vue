<template>
  <div id="container">
    <div id="discard">
      <Card v-if="cards.peek()" v-bind:card="cards.peek()" />
      <div v-else>EMPTY!!</div>
    </div>
    <div>
      {{ cards.getLength() }}
    </div>
  </div>
</template>

<script>
import Card from "./Card";
export default {
  name: "DiscardPile",
  components: {
    Card,
  },
  props: ["cards"],
};
</script>

<style scoped>
#container {
  width: 200px;
  height: 150px;
  border: 2px red dashed;
  text-align: center;
}

#discard {
  display: flex;
  justify-content: center;
  margin:5px;
}
</style>