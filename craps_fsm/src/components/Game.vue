<template>
  <div>
    <div class="alert" v-if="message">
      {{ message }}
    </div>
    <div>Streak: {{ streak }}</div>
    <div v-if="gameState == 'play_game'">
      <div id="rolls">
        <div class="dice" v-for="(d, index) in dice" :key="index">
          {{ d }}
        </div>
      </div>
      <div id="dice-total">{{ total }}</div>
      <div>
        <label>Place bet: $({{ money }}) left</label><br />
        <input type="text" v-model="bet" placeholder="Place your bet" />
        <button @click="roll">Roll</button>
        <button>Withdraw</button>
      </div>
    </div>
    <div v-if="gameState == 'game_over'">
      <h1>Too bad you lose everything!</h1>
      <button @click="restartGame">Restart game</button>
    </div>
  </div>
</template>

<script>
export default {
  data: function() {
    return {
      dice: [],
      money: 100,
      history: [],
      gameState: "game_start",
      bet: 0,
      message: "",
      streak: 1,
      hack: false
    };
  },
  created: function() {
    // React -- constructor
    // happens before the first render
    // loading from axios
    // initalizing data to default values
    this.setupGame();
  },
  mounted: function() {
    // React -- componentDidMount
    // happens after the first render
    // etc.timer
  },
  methods: {
    setupGame: function() {
      //  instead of : this.dice[0] = 1;
      this.$set(this.dice, 0, 1);

      // instead of : this.dice[1] = 1;
      this.$set(this.dice, 1, 1);

      // automatically go to the play game state
      this.gameState = "play_game";

      this.money = 100;
      this.streak = 1;
    },
    rollDice: function() {
      let r = Math.floor(Math.random() * 6) + 1;
      return r;
    },
    roll: function() {
      if (this.bet < 10 || this.bet > this.money) {
        this.message = "Minimal bet is 10 or you exceeded your wallet";
        return;
      }
      // this.dice[0] = Math.floor(Math.random() * 6) + 1;
      this.message = "";
      this.$set(this.dice, 0, this.rollDice());
      this.$set(this.dice, 1, this.rollDice());
      if (this.hack) {
        this.$set(this.dice, 0, 6);
        this.$set(this.dice, 1, 1);
      }
      if (this.total == 7 || this.total == 11) {
        let loss = parseInt(this.bet * (1 + this.streak / 5));
        this.money = this.money - loss;
        this.message = `You lost ${loss} amount of money`;
        this.streak = 1;
        if (this.money <= 0) {
          this.gameState = "game_over";
        } else {
          this.gameState = "play_game";
        }
      } else {
        this.gameState = "play_game";
        this.money += parseInt(this.bet * (1 + this.streak / 10));
        this.streak += 1;
      }
    },
    restartGame: function() {
      this.gameState = "play_game";
    }
  },
  computed: {
    total: function() {
      return this.dice[0] + this.dice[1];
    }
  },
  watch: {
    gameState: function() {

      if (this.gameState == "play_game") {
                  console.log("detect state change to play_game")
        this.setupGame();
      }
    }
  }
};
</script>

<style scoped>
.dice {
  border: 1px solid black;
  width: 50px;
  height: 50px;
  text-align: center;
  font-size: 32px;
}

#rolls {
  display: flex;
  justify-content: space-around;
}

#dice-total {
  display: flex;
  justify-content: center;
  font-size: 46px;
}

.alert {
  background-color: red;
  padding: 20px;
  margin: 20px;
}
</style>
