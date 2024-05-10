<!-- 
be the code master or the game player
click on buttons to reveal colors
pick which team you are on
sync across browsers so we are all playing the same game
show instructions?
 -->

<script>
  import { onMount } from "svelte";
  import { initializeApp } from "firebase/app";
  import {
    getFirestore,
    collection,
    getDoc,
    doc,
    onSnapshot,
    setDoc,
  } from "firebase/firestore";

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

  onMount(async () => {
    // get a deck
    let deck = [
      {suit: "D", number: 2},{suit: "D",number: 3},{suit: "D", number:4},{suit: "D", number:5},{suit: "D", number:6},{suit: "D", number:7},{suit: "D", number:8},{suit: "D", number:9},{suit: "D", number:10},{suit: "D", number:11},{suit: "D", number:12},{suit: "D", number:13},{suit: "D", number: 14},
      {suit: "H", number: 2},{suit: "H", number: 3},{suit: "H", number: 4},{suit: "H", number: 5},{suit: "H", number: 6},{suit: "H", number: 7},{suit: "H", number: 8},{suit: "H", number: 9},{suit: "H", number: 10},{suit: "H", number: 11},{suit: "H", number: 12},{suit: "H", number: 13},{suit: "H", number: 14},
      {suit: "S", number: 2},{suit: "S", number: 3},{suit: "S", number: 4},{suit: "S", number: 5},{suit: "S", number: 6},{suit: "S", number: 7},{suit: "S", number: 8},{suit: "S", number: 9},{suit: "S", number: 10},{suit: "S", number: 11},{suit: "S", number: 12},{suit: "S", number: 13},{suit: "S", number: 14},
      {suit: "C", number: 2},{suit: "C", number: 3},{suit: "C", number: 4},{suit: "C", number: 5},{suit: "C", number: 6},{suit: "C", number: 7},{suit: "C", number: 8},{suit: "C", number: 9},{suit: "C", number: 10},{suit: "C", number: 11},{suit: "C", number: 12},{suit: "C", number: 13},{suit: "C", number: 14}
    ]
    let hands = [[],[],[],[]];
    for (let i = 0; i < hands.length; i++) {
      for (let j = 0; j < 13; j++) {
        let index = Math.floor(Math.random()*deck.length);
        hands[i].push(deck[index]);
        deck.splice(index, 1);
      }
    }
    console.log(hands);
    
  });
</script>

<main>
  <h1>Hearts Online Arena</h1>

  <div class="trick"></div>
  <div class="hand">
    
  </div>
</main>

<style>
  /* .grid {
    display: grid;
    grid-template-columns: repeat(5, 1fr);
    gap: 10px;
  }
  .grid-item {
    border: 1px solid black;
    border-radius: 5px;
    padding: 10px;
    text-align: center;
    transition: background-color 0.5s;
    background-color: grey;
    color: white;
  }
  .CLICKED {
    border: 3px;
    font-weight: bolder;
    opacity: 50%;
  }
  .BLUE {
    background-color: blue;
  }
  .RED {
    background-color: red;
  }
  .NEUTRAL {
    background-color: darkkhaki;
  }
  .ASSASSIN {
    background-color: black;
  } */
</style>
