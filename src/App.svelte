
<script>
  import { onMount } from "svelte";
  // import { initializeApp } from "firebase/app";
  // import {
  //   getFirestore,
  //   collection,
  //   getDoc,
  //   doc,
  //   onSnapshot,
  //   setDoc,
  // } from "firebase/firestore";

  // let wordGrid = [];
  // const boardsize = 5;
  // let isSpymaster = false;
  // let secretGrid = [];
  // let statusGrid = [];
  // let isLoaded = false;

  // const firebaseConfig = {
  //   apiKey: "AIzaSyAZAb-j5x4Oh0STIRCRs1ssIHbdWUCyNyg",
  //   authDomain: "codenames-webe-class.firebaseapp.com",
  //   projectId: "codenames-webe-class",
  //   storageBucket: "codenames-webe-class.appspot.com",
  //   messagingSenderId: "266546182911",
  //   appId: "1:266546182911:web:20fbf477fbd6f4e98f2aca",
  // };

  // const db = getFirestore(initializeApp(firebaseConfig));

  let hands = [[], [], [], []];
  let currentPlayer = -1;
  let currentState;
  const PASSING = 100;
  onMount(async () => {
    // get a deck
    const deck = [
      {suit: "D", number: 2},{suit: "D",number: 3},{suit: "D", number:4},{suit: "D", number:5},{suit: "D", number:6},{suit: "D", number:7},{suit: "D", number:8},{suit: "D", number:9},{suit: "D", number:10},{suit: "D", number:11},{suit: "D", number:12},{suit: "D", number:13},{suit: "D", number: 14},
      {suit: "H", number: 2},{suit: "H", number: 3},{suit: "H", number: 4},{suit: "H", number: 5},{suit: "H", number: 6},{suit: "H", number: 7},{suit: "H", number: 8},{suit: "H", number: 9},{suit: "H", number: 10},{suit: "H", number: 11},{suit: "H", number: 12},{suit: "H", number: 13},{suit: "H", number: 14},
      {suit: "S", number: 2},{suit: "S", number: 3},{suit: "S", number: 4},{suit: "S", number: 5},{suit: "S", number: 6},{suit: "S", number: 7},{suit: "S", number: 8},{suit: "S", number: 9},{suit: "S", number: 10},{suit: "S", number: 11},{suit: "S", number: 12},{suit: "S", number: 13},{suit: "S", number: 14},
      {suit: "C", number: 2},{suit: "C", number: 3},{suit: "C", number: 4},{suit: "C", number: 5},{suit: "C", number: 6},{suit: "C", number: 7},{suit: "C", number: 8},{suit: "C", number: 9},{suit: "C", number: 10},{suit: "C", number: 11},{suit: "C", number: 12},{suit: "C", number: 13},{suit: "C", number: 14}
    ]
    for (let i = 0; i < hands.length; i++) {
      for (let j = 0; j < 13; j++) {
        let index = Math.floor(Math.random()*deck.length);
        hands[i].push(deck[index]);
        deck.splice(index, 1);
      }
    }
    hands = hands;
    
  });

  let sortHand = (val) => {
    const oldHand = hands[val].slice();
    let newHand = [];
    const suits = ["C", "S", "D", "H"];
    suits.forEach(suit => {
      let cards = []
      for (let i = 0; i < oldHand.length; i++) {
        if (oldHand[i].suit === suit) {
          cards.push(oldHand[i]);
          oldHand.splice(i, 0);
        }
      }
      cards = cards.sort((a, b) => a.number - b.number)
      newHand = newHand.concat(cards)
    });
    hands[val] = newHand;
  };
</script>

<main>
  <h1>Hearts Online Arena</h1>

  <div class="trick"></div>
  <h3>Pick a hand:</h3>
  <div class="playerselect">
  {#each [0, 1, 2, 3, 4] as val}
    <button class="handbutton" on:click={() => {currentPlayer = val-1;}}>{val}</button>
  {/each}
  </div>
  {#if currentPlayer >= 0}
    <h3>Your Hand (player {currentPlayer+1}):</h3>
    <div class="hand">
      {#each hands[currentPlayer] as card}
        <button class="grid-item {card.suit === "S" || card.suit === "C" ? "black" : "red" }"

        >
          {card.number} {card.suit}
        </button>
      {/each}
    </div>
    <br>
    <button
      on:click={() => {sortHand(currentPlayer)}}
    >Sort Hand</button>
  {/if}
</main>

<style>
  .hand {
    display: grid;
    grid-template-columns: repeat(5, 1fr);
    gap: 10px;
  }
  .grid-item {
    background-color: aliceblue;
    border: 2px solid navy;
  }
  .black {
    color: black;
  }
  .red {
    color: red
  }
  .handbutton {
    margin: 5px;
  }
</style>
