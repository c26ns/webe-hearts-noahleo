<script>
  import { onMount } from "svelte";

  let wordGrid = [];
  const boardsize = 5;

  onMount(() => {
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
      });
  });
</script>

<main>
  <h1>WebE Codenames</h1>

  <div class="grid">
    {#each wordGrid as row}
      {#each row as word}
        <button class="grid-item">{word}</button>
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
</style>
