<script lang="ts">
  // * Constants
  type Piece = '🔴' | '⚫' | '___'
  const BLANK = '___'
  const BOARD_WIDTH = 7
  const BOARD_HEIGHT = 6
  const emptyRow = () => new Array<Piece>(BOARD_WIDTH).fill(BLANK)

  // * Game State
  let board = new Array<Array<Piece>>(BOARD_HEIGHT).fill(emptyRow())
  let userTurn = true
  let gameOver = false
  let userWon = false

  const reset = (): void => {
    board = new Array<Array<Piece>>(BOARD_HEIGHT).fill(emptyRow())
    userTurn = true
    gameOver = false
    userWon = false
  }

  const handleDropPiece = (piece: Piece, column: number): void => {
    dropPiece(piece, column)
    checkGameOver()
    computerTurn()
    checkGameOver()
  }

  // * Drops the given piece at the selected column if possible
  const dropPiece = (piece: Piece, column: number): void => {
    let currentRow = 0
    if (board[currentRow][column] !== BLANK) return // Column is full

    while (currentRow < BOARD_HEIGHT && board[currentRow][column] === BLANK) {
      currentRow++
    }

    board = [
      ...board.slice(0, currentRow - 1),
      board[currentRow - 1].map((p: Piece, i: number) => (i === column ? piece : p)),
      ...board.slice(currentRow),
    ]

    userTurn = false
  }

  const checkGameOver = () => {
    if (isGameOver()) {
      gameOver = true
      userWon = !userTurn
    }
  }

  const isGameOver = (): boolean => {
    return false
  }

  const computerTurn = (): void => {
    // * Random strat
    let randCol = Math.floor(Math.random() * Math.floor(BOARD_WIDTH))
    dropPiece('⚫', randCol)
    userTurn = true
  }
</script>

<style>
  .centered {
    margin: auto;
    width: 50%;
    /* border: 3px solid lightblue; */
    padding: 10px;
  }

  td {
    padding: 12px;
  }
  button {
    margin: 12px;
  }
</style>

<div class="centered">

  <h3>{gameOver ? (userWon ? 'You won!' : 'You lost!') : ''}</h3>
  <button on:click={reset}>Reset</button>

  <table class="centered">
    <tr>
      {#each emptyRow() as item, i}
        <button on:click={() => handleDropPiece('🔴', i)} disabled={!userTurn}>🌟</button>
      {/each}
    </tr>
  </table>
  <table class="centered">
    {#each board as row}
      <tr>
        {#each row as piece}
          <td>{piece}</td>
        {/each}
      </tr>
    {/each}
  </table>
</div>
