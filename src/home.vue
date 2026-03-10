<script setup>
const cardInfo = [
    {
        title: "Chess",
        desc: "Two players maneuver distinct pieces across a grid to checkmate the king.",
        image: "chess.jpg"
    },
    {
        title: "Checkers",
        desc: "Players move diagonal pieces to capture the opponent by jumping over them.",
        image: "checkers.png"
    },
    {
        title: "Tic-Tac-Toe",
        desc: "Players take turns marking a 3×3 grid, trying to get three in a row.",
        image: "tictactoe.jpg"
    },
    {
        title: "Connect 1 Less than 5",
        desc: "Players drop discs into a vertical grid, aiming to form a line of one less than five.",
        image: "connect4.PNG"
    },
    {
        title: "Super Tic-Tac-Toe",
        desc: "A big Tic‑Tac‑Toe made of smaller boards where each move decides which mini‑board the next player must use.",
        image: "supertictactoe.png"
    },
    {
        title: "Othello",
        desc: "Players flip opponent discs by trapping them between their own pieces.",
        image: "othello.jpg"
    },
]
</script>

<template>
    <v-container class="mt-6">
        <v-row>
            <v-col
                v-for="item in cardInfo"
                :key="item.title"
                cols="12"
                sm="6"
                md="4"
            >
                <v-card border="sm" color="grey-darken-3" class="quantico-bold">
                    <v-img :src="item.image" height="500px" cover/>
                    <v-card-title>{{ item.title }}</v-card-title>
                    <v-card-subtitle>{{ item.desc }}</v-card-subtitle>
                    <v-card-text>{{ item.description }}</v-card-text>
                    <v-btn class="ml-5 mb-5">
                        <v-icon start>mdi-play-circle-outline</v-icon>
                        Play!
                    </v-btn>
                </v-card>
            </v-col>
        </v-row>
    </v-container>
</template>

<style scoped>
.quantico-regular {
    font-family: "Quantico", sans-serif;
    font-weight: 400;
    font-style: normal;
}

.quantico-bold {
    font-family: "Quantico", sans-serif;
    font-weight: 700;
    font-style: normal;
}

.quantico-regular-italic {
    font-family: "Quantico", sans-serif;
    font-weight: 400;
    font-style: italic;
}

.quantico-bold-italic {
    font-family: "Quantico", sans-serif;
    font-weight: 700;
    font-style: italic;
}
</style>