<template>
  <div>
    <h1>UNO</h1>
    <h2>{{players[activePlayer].player}}</h2>
    <div id="hand">
        <div class="card" v-for="(card,index) in activePlayerHand" :key='index' :style="{backgroundColor: card.suit}">
            {{card.value}} 
        </div>
    </div>
  </div>
</template>

<script>
import Stack from "../data-structures/Stack";
export default {
  data: function () {
    return {
      deck: new Stack(),
      numberOfPlayers:2,
      players: [],
      round: 0,
      activePlayer: 0
    };
  },
  methods: {
    initDeck: function () {
      let colors = ["red", "yellow", "blue", "green"];
      let cards = [];
      for (let c of colors) {
        // for each color

        // push 0 onto the cards as well
        cards.push({
          value: 0,
          suit: c,
        });

        for (let i = 1; i <= 9; i++) {
          // for each number from 1 to 9
          for (let j = 0; j < 2; j++) {
            // put that card onto the deck
            cards.push({
              value: i,
              suit: c,
            });
          }
        }
      }

      for (let s = 0; s < 20000; s++) {
        let firstIndex = Math.floor(Math.random() * cards.length);
        let secondIndex = Math.floor(Math.random() * cards.length);
        let temp = cards[firstIndex];
        cards[firstIndex] = cards[secondIndex];
        cards[secondIndex] = temp;
      }

      for (let card of cards) {
        this.deck.push(card);
      }
    },
  },
  computed:  {
      activePlayerHand:function() {
          return this.players[this.activePlayer].hand
      }
  },
  mounted: function () {
      this.initDeck();

      for (let i =0; i < this.numberOfPlayers; i++) {
          let hand = [];
          for (let i=0; i < 7; i++) {
            hand.push(this.deck.pop());

          }
          this.$set(this.players, i, {
              'player': `Player ${i+1}`,
              'hand':hand
          })
      }
  },
};
</script>

<style scoped>
    .card {
        border: 1px black solid;
        height: 110px;
        width: 60px;
        border-radius: 10px;
        text-align:center;
        font-size: 24px;
 
    }

    #hand {
        display: flex;
        justify-content: space-evenly;
    }

</style>
