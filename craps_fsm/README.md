# craps

## Rules of the game

1. Player place bet
2. Player rolls the dice.
3. If the dice is exactly 7, player loses
4. If not, the player wins
5. Ask the player: go for another round?

## Extract out the "state"
The state refers to the variables that you need to model the problem.
Vue has 
* data: this is for variables and *data storage*
* props: *arguments* that is passed in from another component (known as parent component)
* computed: data that is constantly re-evaluated automatic when changes to prop and data is detected
* methods: function calls

React has:
* state: this is for variables and *data storage*
* props: *arguments* that is constantly re-evaluared automatically when changes to prop and data is detected

Note: we can always call a function in JSX.  Calling a functions in JSX is the same `computed` and `methods` in Vue.

### What is the state
* Any data (i.e variable) that we need to do the game logic is part of the state. 
* Anything that is displayed to user must be data or computed. 
* We use computed if it is derived. (We can take existing data and calculate it) -- if in React, there is no computed,
  we can use a function instead and call that function in the JSX.

### Craps game state
* dice
* money
* times
* history of rolls

## DESIGN THE INTERACTION
What events will cause changes to the state?

* The user presses "Roll" button, we will roll the dice and check if it is 7
* The user can choose to "Withdraw", and the game will end

## WHAT ARE THE FINITE STATES

1. Game start state:
   - user has 100 cash
   - dice is set to 1, 1 respectively
   - automatically go to the game play state
 

2. Play game state
     - user choose to roll dice or Withdraw
   - if user choose to roll dice:
      - determine if the player wins, if player wins, go back to the play game state
      - if player looses, go to the game over state
   - if user chooe to Withdraw: end game state

3. Game over state

4. End game state