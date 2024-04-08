<script>
    export let size = 3
    export let totem = 'X'
    let board = Array(size).fill().map(() => Array(size).fill(null));
    //[ [ null, null, null ],
    //  [ null, null, null ],
    //  [ null, null, null ] ]
    let activeCells = 0

    function cellClickHander(row, cell) {
        
        board[row][cell] = totem

        totem = totem === 'X' ? 'O' : 'X';
        activeCells++
        console.log(winCheck())
        console.log(board)
    }

    function winCheck() {
        let rowWin = board.some((row) => {
            return row.every(cell => cell === row[0] && cell !== null)
        })

        let diaWin = board.every((row, i) => {
            return (row[i] === board[0][0]) || (row[i] === board[0][2])
        })

        if (rowWin) return rowWin
        if (diaWin) return diaWin
        return false
    }
</script>   

<style>
    .board {
      display: grid;
      grid-template-columns: repeat(var(--size), 1fr);
      gap: 5px;
      justify-items: center;
      max-width: 400px;
      margin: 100px auto;
      border-collapse: collapse;
    }
    
    .cell {
      width: 100%;
      height: 100%;
      background-color: #f0f0f0;
      display: flex;
      justify-content: center;
      align-items: center;
      cursor: pointer;
      border: 1px solid black;
      aspect-ratio: 1 / 1;
    }
</style>

<div class="board" style="--size: {size};">
    {#each board as row, ri}
        {#each row as cell, ci}
            <!-- svelte-ignore a11y-no-static-element-interactions -->
            <!-- svelte-ignore a11y-click-events-have-key-events -->
            <div class="cell" id={`${row}+${cell}`} on:click={() => cellClickHander(ri, ci)} >
                {cell}
            </div>
        {/each}
    {/each}
</div>