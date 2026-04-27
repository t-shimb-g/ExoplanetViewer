<script setup>
import { ref, computed } from 'vue'
import home from './home.vue'
import games from './games.vue'

const routes = {
    '/': home,
    '/games': games
}

const currentPath = ref(window.location.hash)
const drawer = ref(false);

window.addEventListener('hashchange', () => {
    currentPath.value = window.location.hash
})

const currentView = computed(() => {
    return routes[currentPath.value.slice(1) || '/'] || NotFound
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
                prepend-icon="mdi-information-variant-circle-outline"
                href="#/games"
                title="Games"
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