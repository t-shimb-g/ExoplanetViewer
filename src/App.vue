<script setup>
import { ref, computed } from 'vue'
import home from './home.vue'
import about from './about.vue'

const routes = {
    '/': home,
    '/about': about,
}

const currentPath = ref(window.location.hash)
const drawer = ref(true);

window.addEventListener('hashchange', () => {
    currentPath.value = window.location.hash
})

const currentView = computed(() => {
    return routes[currentPath.value.slice(1) || '/'] || NotFound
})

</script>

<template>
    <v-app>
        <v-navigation-drawer color="blue" v-model="drawer">
            <v-list-item
                prepend-icon="mdi-home"
                href="#/"
                title="Home"
                @click="drawer = !drawer"
            ></v-list-item>
            <v-list-item
                prepend-icon="mdi-information-variant-circle-outline"
                href="#/about"
                title="About Us"
                @click="drawer = !drawer"
            ></v-list-item>
        </v-navigation-drawer>
        <v-app-bar>
            <v-app-bar-nav-icon @click="drawer = !drawer"></v-app-bar-nav-icon>
            <v-app-bar-title>Exoplanet Explorer</v-app-bar-title>
        </v-app-bar>
        <v-main>
            <component :is="currentView"></component>
        </v-main>
        <v-footer app="true">Copyright 2026</v-footer>
    </v-app>
</template>