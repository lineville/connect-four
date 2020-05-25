<script lang="ts">
  // * Constants
  enum Piece {
    Red = '🔴',
    Black = '⚫',
    Empty = '___',
  }
  const BOARD_WIDTH = 7
  const BOARD_HEIGHT = 6
  const emptyRow = () => new Array<Piece>(BOARD_WIDTH).fill(Piece.Empty)

  // * Game State
  let board = new Array<Array<Piece>>(BOARD_HEIGHT).fill(emptyRow())
  let userTurn = true
  let gameOver = false
  let userWon = false

  // * Resets the game state
  const reset = (): void => {
    board = new Array<Array<Piece>>(BOARD_HEIGHT).fill(emptyRow())
    userTurn = true
    gameOver = false
    userWon = false
  }

  // * Handles the user selecting a column to drop piece
  const handleDropPiece = (piece: Piece, column: number): void => {
    dropPiece(piece, column)
    checkGameOver()
    computerTurn()
    checkGameOver()
  }

  // * Drops the given piece at the selected column if possible
  const dropPiece = (piece: Piece, column: number): void => {
    let currentRow = 0
    if (board[currentRow][column] !== Piece.Empty) return // Column is full

    while (currentRow < BOARD_HEIGHT && board[currentRow][column] === Piece.Empty) {
      currentRow++
    }

    board = [
      ...board.slice(0, currentRow - 1),
      board[currentRow - 1].map((p: Piece, i: number) => (i === column ? piece : p)),
      ...board.slice(currentRow),
    ]

    userTurn = false
  }

  // * Checks if game has ended and sets appropriate flags
  const checkGameOver = () => {
    if (isGameOver()) {
      gameOver = true
      userWon = !userTurn
    }
  }

  // * Checks if there exists a four-streak horizontal, vertical or diagonal
  const isGameOver = (): boolean => {
    return false
  }

  // * Choses which column to place black piece on board
  const computerTurn = (): void => {
    let randCol = Math.floor(Math.random() * Math.floor(BOARD_WIDTH))
    dropPiece(Piece.Black, randCol)
    userTurn = true
  }
</script>

<style>
  .centered {
    margin: auto;
    width: 50%;
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
        <button on:click={() => handleDropPiece(Piece.Red, i)} disabled={!userTurn}>🌟</button>
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
