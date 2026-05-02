<script setup>
import { ref } from 'vue'

const width = 7
const height = 6
const gridSize = width * height

const board = ref(Array(gridSize).fill(''))
const currentPlayer = ref('Red')

const winner = ref(null)
const winningLine = ref([])
let winArray = null

function makeMove(i) {
    if (winner.value !== null) return

    const col = (i % width) + 1
    let row = height

    while (row > 0) {
        let space = ( (row*width) + col ) - 1
        if (board.value[space] === '') {
            board.value[space] = currentPlayer.value
            break
        }
        --row
    }

    winArray = checkWinner()
    console.log(winArray)
    if (winArray !== null) {
        winner.value = winArray.player
        winningLine.value = winArray.line
    }
    currentPlayer.value = currentPlayer.value === 'Red' ? 'Yellow' : 'Red'
}

function checkDirection(start, direction) {
    // from start, checks in specified direction
    // returns player and winning line (or null if there is no winner)
    const player = board.value[start]
    if (!player) { // empty space
        return null
    }

    const line = [start]

    for (let i = 1; i < 4; ++i) {
        const next = start + direction * i
        if (next < 0 || next >= gridSize) { return null }
        if (board.value[next] !== player) { return null }
        line.push(next)
    }
    return { player, line }
}

function checkWinner() {
    for (let i = gridSize - 1; i >= 0; --i) {
        const col = (i % width) + 1
        const row = Math.floor(i / width) + 1

        if (!board.value[i]) continue

        // horizontal
        if (col > 3) {
            const win = checkDirection(i, -1)
            if (win !== null) return win
        }
        // vertical
        if (row > 3) {
            const win = checkDirection(i, -width)
            if (win !== null) return win
        }
        // up left diagonal
        if (row > 3 && col > 3) {
            const win = checkDirection(i, -width - 1)
            if (win !== null) return win
        }
        // up right diagonal
        if (row > 3 && col < 5) {
            const win = checkDirection(i, -width + 1)
            if (win !== null) return win
        }
    }
    return null
}

function reset() {
    board.value = Array(gridSize).fill('')
    winner.value = null
    winningLine.value = []
    currentPlayer.value = 'Red'
}
</script>

<template>
    <h1 class="audiowide-regular ma-5">Connect 1 Less Than 5</h1>

    <v-container class="mt-6" style="width: 750px">
        <h2
            v-if="winner === null"
            class="quantico turn-indicator d-flex mx-auto"
        >
            {{ currentPlayer }}'s turn
        </h2>
        <h2
            v-if="winner !== null"
            class="quantico turn-indicator d-flex mx-auto"
        >
            {{ winner }} wins!
        </h2>
        <v-row>
            <v-col
                v-for="(cell, i) in board"
                :key="i"
                @click="makeMove(i)"
                cols=""
            >
                <div
                    class="cell"
                    :class="winningLine.includes(i) ? 'win-' + cell.toLowerCase() : 'cell-' + cell.toLowerCase()"
                />
            </v-col>
        </v-row>
        <v-btn @click="reset" class="audiowide-regular reset-btn d-flex mx-auto">Reset</v-btn>
    </v-container>
</template>

<style scoped>
.quantico {
    font-family: "Quantico", sans-serif;
    font-weight: 700;
    font-style: normal;
}

.cell {
    width: 80px;
    height: 80px;
    border: 10px solid #ccc;
    border-radius: 40px;
    font-size: 50px;
    display: flex;
    align-items: center;
    justify-content: center;
}

.cell-red {
    background: red;
}

.cell-yellow {
    background: yellow;
}

.win-red {
    background: red;
    border-color: darkred;
}

.win-yellow {
    background: yellow;
    border-color: #ffb700;
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