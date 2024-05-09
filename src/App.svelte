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

  let wordGrid = [];
  const boardsize = 5;
  let isSpymaster = false;
  let secretGrid = [];
  let statusGrid = [];
  let isLoaded = false;

  const firebaseConfig = {
    apiKey: "AIzaSyAZAb-j5x4Oh0STIRCRs1ssIHbdWUCyNyg",
    authDomain: "codenames-webe-class.firebaseapp.com",
    projectId: "codenames-webe-class",
    storageBucket: "codenames-webe-class.appspot.com",
    messagingSenderId: "266546182911",
    appId: "1:266546182911:web:20fbf477fbd6f4e98f2aca",
  };

  const db = getFirestore(initializeApp(firebaseConfig));

  onMount(async () => {
    //get the board from the database:
    const gameDoc = doc(db, "codenames", "game");
    const gameSnap = await getDoc(gameDoc);

    if (gameSnap.exists()) {
      const gameData = gameSnap.data();
      wordGrid = JSON.parse(gameData.wordGrid);
      secretGrid = JSON.parse(gameData.secretGrid);
      statusGrid = JSON.parse(gameData.statusGrid);
    } else {
      buildBoard();
    }
    isLoaded = true;

    onSnapshot(gameDoc, (gameSnap) => {
      const gameData = gameSnap.data();
      wordGrid = JSON.parse(gameData.wordGrid);
      secretGrid = JSON.parse(gameData.secretGrid);
      statusGrid = JSON.parse(gameData.statusGrid);
    });
  });

  $: {
    if (isLoaded) {
      const gameDoc = doc(db, "codenames", "game");
      setDoc(gameDoc, {
        wordGrid: JSON.stringify(wordGrid),
        secretGrid: JSON.stringify(secretGrid),
        statusGrid: JSON.stringify(statusGrid),
      });
    }
  }

  function buildBoard() {
    isLoaded = false;
    //BUILD THE WORD BOARD:
    fetch("wordlist.txt")
      .then((response) => response.text())
      .then((text) => {
        const words = text.split("\n");

        for (let i = 0; i < boardsize; i++) {
          let row = [];
          for (let j = 0; j < boardsize; j++) {
            let randomIndex = Math.floor(Math.random() * words.length);
            row.push(words[randomIndex]);
            words.splice(randomIndex, 1);
          }
          wordGrid.push(row);
          wordGrid = wordGrid;
        }
        isLoaded = true;
      });

    //BUILD THE SECRET GRID:
    let colors = Array(9)
      .fill("RED")
      .concat(Array(8).fill("BLUE"))
      .concat(Array(7).fill("NEUTRAL"))
      .concat(["ASSASSIN"]);
    for (let i = 0; i < boardsize; i++) {
      let row = [];
      for (let j = 0; j < boardsize; j++) {
        let randomIndex = Math.floor(Math.random() * colors.length);
        row.push(colors[randomIndex]);
        colors.splice(randomIndex, 1);
      }
      secretGrid.push(row);
      secretGrid = secretGrid;
    }

    //BUILD THE STATUS GRID:
    statusGrid = Array(boardsize)
      .fill()
      .map(() => Array(boardsize).fill("UNCLICKED"));
  }
</script>

<main>
  <h1>WebE Codenames</h1>

  <input type="checkbox" bind:checked={isSpymaster} />
  <button on:click={buildBoard}>New Game</button>
  Spymaster

  <div class="grid">
    {#each wordGrid as row, i}
      {#each row as word, j}
        <button
          on:click={() => (statusGrid[i][j] = "CLICKED")}
          class="grid-item {isSpymaster || statusGrid[i][j] === 'CLICKED'
            ? secretGrid[i][j]
            : ''}
            {statusGrid[i][j]}">{word}</button
        >
      {/each}
    {/each}
  </div>
</main>

<style>
  .grid {
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
  }
</style>
