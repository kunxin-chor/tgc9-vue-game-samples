<template>
  <div>
    <AskForNames v-if="gameState == 'ask_for_names'"
        @beginGameEvent="beginGame"
    
    />  
    <div id="game" v-if="gameState == 'player1_turn' || gameState=='player2_turn'">
      <h1>UNO</h1>
      <h2>{{ players[activePlayer].player }}</h2>
      <div id="discard-pile-container">
        <DiscardPile v-bind:cards="discard" />
      </div>
      <div id="hand">
        <Card
          v-for="(c, index) in activePlayerHand"
          :key="index"
          v-bind:card="c"
          v-bind:canDiscard="checkCanDiscard(c)"
          @discardCardEvent="processDiscardCard"
        />
      </div>
      <button id="end-turn" @click="endTurn">End Turn</button>
    </div>
  </div>
</template>

<script>
import Stack from "../data-structures/Stack";
import Card from "./Card";
import DiscardPile from "./DiscardPile";
import AskForNames from "./AskForNames";
export default {
  components: {
    Card,
    DiscardPile,
    AskForNames,
  },
  data: function () {
    return {
      deck: new Stack(),
      numberOfPlayers: 2,
      players: [],
      round: 0,
      activePlayer: 0,
      discard: new Stack(),
      gameState: "",
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
    checkCanDiscard: function (card) {
      // a card will be a key-value pair of suit and color
      // example:
      // {
      //  value: 3,
      //  suit: 'red'
      // }
      let topCard = this.discard.peek();
      if (card.suit == topCard.suit || card.value == topCard.value) {
        return true;
      } else {
        return false;
      }
    },
    processDiscardCard: function (card) {
      // remove the card from player's hand
      let indexToRemove = this.activePlayerHand.findIndex((c) => {
        return c.value === card.value && c.suit === card.suit;
      });
      this.activePlayerHand.splice(indexToRemove, 1);

      this.discard.push(card);
    },
    takeCard: function (numberOfCards) {
      for (let i = 0; i < numberOfCards; i++) {
        let topCard = this.deck.pop();
        this.activePlayerHand.push(topCard);
      }
    },
    endTurn: function () {
      this.gameState = "end_turn";
    },
    beginGame:function(player1Name, player2Name) {
        this.players[0].player = player1Name;
        this.players[1].player = player2Name;
        this.gameState = 'player1_turn';

    }
  },
  computed: {
    activePlayerHand: function () {
      return this.players[this.activePlayer].hand;
    },
  },
  created: function () {
    this.initDeck();

    this.discard.push({
      value: 1,
      suit: "red",
    });

    for (let i = 0; i < this.numberOfPlayers; i++) {
      let hand = [];
      for (let i = 0; i < 7; i++) {
        hand.push(this.deck.pop());
      }
      this.$set(this.players, i, {
        player: `Player ${i + 1}`,
        hand: hand,
      });
    }

    this.gameState = "ask_for_names";
  },
  watch: {
    gameState: function () {
      if (this.gameState === "player1_turn") {
        this.activePlayer = 0;
        this.takeCard(1);
      }
      if (this.gameState === "player2_turn") {
        this.activePlayer = 1;
        this.takeCard(1);
      }
      if (this.gameState === "end_turn") {
        alert("Your turn is over");
        if (this.activePlayer === 0) {
          this.gameState = "player2_turn";
        } else {
          this.gameState = "player1_turn";
        }
      }
    },
  },
};
</script>

<style scoped>
#hand {
  display: flex;
  justify-content: space-evenly;
}

#discard-pile-container {
  display: flex;
  justify-content: center;
  margin: 25px;
}

#end-turn {
  margin: 25px;
}
</style>
