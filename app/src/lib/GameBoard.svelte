<script>
    export let size
    export let totem
    let activeCells
    let board = []

    function cellClickHander(row, cell) {
        if (board[row][cell] !== null) {return}
        
        board[row][cell] = totem

        activeCells++
        console.log(winCheck(row, cell))
        console.log(board)
        totem = totem === 'X' ? 'O' : 'X';
    }

    function winCheck(ri, ci) {
        //Check rowwise
        let rowTest = board.some((row)=>{
            return row.every(cell => cell === row[0] && cell !== null)
        })

        //Check colwise (colonoscopy hahah)
        let colTest = (ref) => {
            if (ref > 2) return false

            let test = board.every((row, i)=>{
                return (row[ref] === board[0][ref]) && (row[ref] !== null)
            })

            if (test == true) return true
            
            return colTest(ref+1)
        }
        
        if (rowTest == true) return 'Row wins'
        if (colTest(0) == true) return 'Col wins'
        
        
        
        
        
        
        
        // if (activeCells <= 2) return "No winner yet"

        // let rowWin = board.some((row) => {
        //     return row.every(cell => cell === row[0] && cell !== null)
        // })

        // let diaWin = board.every((row, i) => {
        //     return (row[i] === board[0][0]) || (row[i] === board[0][2])
        // })

        // let verWin = board.every((row, i) => {
        //     return board.every()
        // })

        // if (rowWin == true) return `${totem} wins in a row!`
        // if (diaWin == true) return `${totem} wins in diagonal`
        // return false
    }

    function _initBoard() {
        size = 3
        totem = 'X'
        activeCells = 0

        board =  Array(size).fill().map(() => Array(size).fill(null));

        //[ [ null, null, null ],
        //  [ null, null, null ],
        //  [ null, null, null ] ]
    }

    _initBoard()
</script>   

<style>
    .board {
      display: grid;
      grid-template-columns: repeat(var(--size), 1fr);
      gap: 5px;
      justify-items: center;
      max-width: 400px;
      max-height: 400px;
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
      user-select: none;
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

<button on:click={()=>_initBoard()}>Reset</button>