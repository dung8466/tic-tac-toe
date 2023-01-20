<script>
import { ref, computed, watch } from 'vue'
const calculateWinner = board => {
    const lines = [
        [0, 1, 2],
        [3, 4, 5],
        [6, 7, 8],
        [0, 4, 8],
        [0, 3, 6],
        [1, 4, 7],
        [2, 5, 8],
        [2, 4, 6]
    ]
    for (let i = 0; i < lines.length; i++) {
        const [a, b, c] = lines[i]
        if (board[a] && board[a] === board[b] && board[a] === board[c]) {
            return board[a]
        }
    }
    return null
}

export default {
    setup() {
        const xWin = ref(0)
        const oWin = ref(0)
        const player = ref('X')
        const board = ref([
            ['', '', ''],
            ['', '', ''],
            ['', '', '']
        ])
        const winner = computed(() => calculateWinner(board.value.flat()))
        const move = (x, y) => {
            if (winner.value) {
               return
            }
            else{
                if (board.value[x][y] === '') {
                    board.value[x][y] = player.value
                    player.value = player.value === 'O' ? 'X' : 'O'
                }
            }
        }
        watch(winner, () => {
            if(winner.value === "X"){
                xWin.value++
            }
            if(winner.value === "O"){
                oWin.value++
            }
        })
        const reset = () => {
            player.value = 'X'
            board.value = [
                ['', '', ''],
                ['', '', ''],
                ['', '', '']

            ]
        }
        return { winner, player, board, move, reset, xWin, oWin }
    },
}
</script>
<template>
    <div class="game">
        <div class="container">
            <h2 v-if='winner'>Winner: {{ winner }}</h2>
            <h2 v-else>Player Move: {{ player }}</h2>
            <div class="board">
                <div v-for="(_, x) in 3" :key="x" class="row">
                    <button v-for="(_, y) in 3" :key="y" class="col" @click="move(x, y)">
                        {{ board[x][y] }}
                    </button>
                </div>
            </div>
            <button @click="reset" class="reset">Reset</button>
        </div>
        <div class="score">
            <div>
                <p>X</p>
                <p>{{ xWin }}</p>
            </div>
            <div>
                <p>O</p>
                <p>{{ oWin }}</p>
            </div>
        </div>
    </div>
</template>
<style scoped>
.game {
    display: flex;
    flex-direction: row;
    align-items: center;
    justify-content: center;
    align-content: space-between;
}

.score {
    display: flex;
    flex-direction: row;
    align-items: center;
    justify-content: center;
    align-content: space-between;
}
.container {
    border: 1px solid #999;
    padding: 1em;
}

.board {
    display: flex;
    align-items: center;
    justify-content: center;
    flex-direction: column;
    margin: 35px;
}

.row {
    display: flex;
    align-items: center;
    justify-content: center;
}

button {
    border: 1px solid black;
    border-radius: 5px;
    height: 75px;
    margin: 2px;
    width: 75px;
    text-align: center;
    font-size: 50px;
    font-weight: 500;
}
.reset {
    font-size: 18px;
    margin-left: 120px;
}
button:hover {
    cursor: pointer;
    background-color: rgb(240, 240, 240);
}
</style>
