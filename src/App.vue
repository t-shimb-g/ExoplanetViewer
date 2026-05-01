<script setup>
import { ref, computed } from 'vue'
import home from './home.vue'
import games from './games.vue'
import favGames from './favGames.vue'

import connect4 from './games/connect4.vue'
import ttt from './games/tictactoe.vue'

const routes = {
    '/': home,
    '/games': games,
    '/favorite-games': favGames
}

const currentPath = ref(window.location.hash)
const drawer = ref(false);

window.addEventListener('hashchange', () => {
    currentPath.value = window.location.hash
})

// const currentView = computed(() => {
//     return routes[currentPath.value.slice(1) || '/'] || NotFound
// })
const gameComponents = { connect4, ttt }

const currentView = computed(() => {
    const path = currentPath.value.slice(1) || '/'
    const segments = path.split('/').filter(Boolean)

    // Nested: /games/:game
    if (segments[0] === 'games' && segments[1]) {
        return gameComponents[segments[1]] || NotFound
    }

    // Top-level routes
    return routes[path] || NotFound
})
</script>

<template>
    <v-app class="bg-black">
        <v-navigation-drawer color="grey-darken-4" v-model="drawer" class="audiowide-regular">
            <v-list-item
                prepend-icon="mdi-home"
                href="#/"
                title="Home"
                @click="drawer = !drawer"
            ></v-list-item>
            <v-list-item
                prepend-icon="mdi-puzzle"
                href="#/games"
                title="Games"
                @click="drawer = !drawer"
            ></v-list-item>
            <v-list-item
                prepend-icon="mdi-puzzle-heart"
                href="#/favorite-games"
                title="Favorite Games"
                @click="drawer = !drawer"
            ></v-list-item>
        </v-navigation-drawer>
        <v-app-bar
            color="indigo-darken-4"
            height="100"
        >
            <v-app-bar-nav-icon @click="drawer = !drawer"></v-app-bar-nav-icon>
            <v-app-bar-title class="audiowide-regular mainHeader">Board Games Galore</v-app-bar-title>
        </v-app-bar>
        <v-main>
            <component :is="currentView"></component>
        </v-main>
    </v-app>
</template>

<style>
.mainHeader {
    line-height: 1.4 !important;
    font-size: 50px !important;
}

.audiowide-regular {
    font-family: "Audiowide", sans-serif;
    font-weight: 400;
    font-style: normal;
}
</style>