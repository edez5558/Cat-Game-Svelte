<script>
  export let squares = Array(9).fill(null);
  export let winner = null;
  export let status = 'Siguiente';
  export let letter = 'X';
  let xIsNext = true;
  let name = 'Svelte';
  
  function handleClick(i) {
    if(winner || squares[i]){
      return;
    }
    
    squares[i] = xIsNext ? 'X' : 'O';
    xIsNext = !xIsNext;

    winner = calculateWinner(squares);

    if(winner){
      status = 'Ganador';
      letter = winner;
    }else{
      status = 'Siguiente';
      letter = xIsNext? 'X' : 'O';
    }
  }
  
  function resetGame() {
    squares = Array(9).fill(null);
    xIsNext = true;

    winner = null;
    status = 'Siguiente';
    letter = 'X';
  }
  
  
  function calculateWinner(squares) {
    const lines = [
      [0, 1, 2],
      [3, 4, 5],
      [6, 7, 8],
      [0, 3, 6],
      [1, 4, 7],
      [2, 5, 8],
      [0, 4, 8],
      [2, 4, 6],
    ];
    for (let i = 0; i < lines.length; i++) {
      const [a, b, c] = lines[i];
      if (squares[a] && squares[a] === squares[b] && squares[a] === squares[c]) {
        return squares[a];
      }
    }
    return null;
  }
</script>

<style>
  :global(body){
    margin: 0;
    font: 14px "Century Gothic", Futura, sans-serif;
  }

  .board-row:after {
    clear: both;
    content: "";
    display: table;
  }

  .status {
    margin-bottom: 10px;
  }

  .square {
    background: #fff;
    border: 2px solid #777272;
    float: left;
    font-size: 30px;
    font-weight: bold;
    line-height: 50px;
    height: 50px;
    margin-right: -1px;
    margin-top: -1px;
    padding: 0;
    text-align: center;
    width: 50px;
  }

  .square:not(.null){
    background-color: #c9c5c5;
  }

  .square:focus {
    outline: none;
  }

  .X,.O{
    text-shadow: 2px 2px 1px rgba(0, 0, 0, 0.5);
  }

  .X{
    color: #ff4242;
  }

  .O{
    color: #95d2ec;
  }

  .kbd-navigation .square:focus {
    background: #ddd;
  }

  .game{
    display: flex;
    flex-direction: row;
    justify-content: space-between;
    align-content: space-between;
    padding: 40px 30px;
  }

  .game-board{
    align-self: center;
    justify-self: center;
  }

  .game-info {
    color: #413c3c;
    align-self: center;
    font-size: 20px;
    font-weight: bold;
  }
  
  h1{
    color: #994747;
  }

  .game-reset{
    align-self: center;
  }

  .Ganador{
    color: #ada44c;
  }

  .Siguiente{
    color: #aa5937;
  }

  button{
    padding: 10px 14px;
    color: #C27664;
    font-size: 30px;
    border: none;
    box-shadow: 1px 1px 1px 2px rgba(0, 0, 0, 0.5);
    cursor: pointer;
  }

  button:hover{
    background-color: #b4afaf;
    color: #e78069;
  }

  .panel{
    background-color: #e4c79d;
    padding: 1px 0px;
    text-align: center;
  }
  .page{
    display: flex;
    flex-direction: column;
    height: 100vh;
  }
</style>


<div class="page">
  <div class="panel">
    <h1>Cat Game '{name}'</h1>
  </div>

  <div class="game">
    <div class="game-info">
      <div class="{status}">{status}<div class="{letter}">{letter}</div></div>
    </div>

    <div class="game-board">
      {#each [0, 1, 2] as row}
        <div class="board-row">
          {#each [0, 1, 2] as col}
            <button
              class="square {squares[row * 3 + col]}"
              on:click={() => handleClick(row * 3 + col)}
            >
              {#if squares[row * 3 + col] != null}
                {squares[row * 3 + col]}
              {/if}
            </button>
          {/each}
        </div>
      {/each}
    </div>

    <div class="game-reset">
      <button on:click={() => resetGame()}>Reiniciar</button>
    </div>
  </div>
</div>