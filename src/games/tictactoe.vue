<script setup>
import { ref } from 'vue'

const board = ref(Array(9).fill(''))
const currentPlayer = ref('X')
const winner = ref(null)
const winningLine = ref([])

const winPatterns = [
    [0,1,2], [3,4,5], [6,7,8], // rows
    [0,3,6], [1,4,7], [2,5,8], // cols
    [0,4,8], [2,4,6]           // diagonals
]

function makeMove(i) {
    if (board.value[i] !== '' || winner.value !== null) return

    board.value[i] = currentPlayer.value
    checkWinner()

    currentPlayer.value = currentPlayer.value === 'X' ? 'O' : 'X'
}

function checkWinner() {
    for (const [a,b,c] of winPatterns) {
        if (board.value[a] !== '' &&
            board.value[a] === board.value[b] &&
            board.value[a] === board.value[c])
        {
            winner.value = board.value[a]
            winningLine.value = [a, b, c]
            return
        }
    }
}

function reset() {
    board.value = Array(9).fill('')
    winner.value = null
    winningLine.value = []
    currentPlayer.value = 'X'
}
</script>

<template>
    <h1 class="audiowide-regular ma-5">Tic Tac Toe</h1>

    <v-container class="mt-6" style="width: 540px">
        <h2
            v-if="winningLine.length === 0"
            class="audiowide-regular turn-indicator d-flex mx-auto"
        >
            {{ currentPlayer }}'s turn
        </h2>
        <h2
            v-if="winningLine.length !== 0"
            class="audiowide-regular turn-indicator d-flex mx-auto"
        >
            {{ winner }} wins!
        </h2>
        <v-row>
            <v-col
                v-for="(cell, i) in board"
                :key="i"
                @click="makeMove(i)"
                cols="4"
            >
                <div
                    class="cell audiowide-regular"
                    :class="winningLine.includes(i) ? 'win-cell' : 'bg-grey-darken-4'"
                >
                    {{ cell }}
                </div>
            </v-col>
        </v-row>
        <v-btn @click="reset" class="audiowide-regular reset-btn d-flex mx-auto">Reset</v-btn>
    </v-container>
</template>

<style scoped>
.cell {
    width: 150px;
    height: 150px;
    border: 10px solid #ccc;
    border-radius: 12px;
    font-size: 50px;
    display: flex;
    align-items: center;
    justify-content: center;
}

.win-cell {
    background: #d4ffd4;
    border-color: #4caf50;
    color: #006600;
}

.reset-btn {
    font-size: 25px;
    margin-top: 20px;
    display: flex;
    align-items: center;
    justify-content: center;
}

.turn-indicator {
    margin-bottom: 20px;
    display: flex;
    align-items: center;
    justify-content: center;
}
</style>