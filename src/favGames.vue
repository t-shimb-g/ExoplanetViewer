<script setup>
import {onMounted, ref} from "vue";
import GameCard from "@/components/gameCard.vue";

const games = ref([])
const favGamesIDs = ref([])
const gameImgs = ref([])

const loading = ref(true)
const error = ref('')
const empty = ref(false)

async function loadFavGames() {
    loading.value = true;
    error.value = '';

    try {
        const favResponse = await fetch('/api/favorites');
        if(!favResponse.ok) {
            throw new Error('Failed to load favorites')
        }
        const favData = await favResponse.json()
        favGamesIDs.value = favData.map(favGame => favGame.id)

        games.value = favData.map((game) => ({
            id: game.id,
            name: game.name,
            desc: game.desc,
            rules: game.rules,
            img: game.img,
            route: game.route,
            enabled: game.enabled,
            favorite: favGamesIDs.value.includes(game.id)
        }))
        if (games.value.length === 0) { empty.value = true }
        gameImgs.value = data.map(game => game.img)

    } catch (err) {
        error.value = err.message || 'Something went wrong while loading games'
    } finally {
        loading.value = false;
    }
}

async function favoriteGame(game_id) {
    const favResponse = await fetch(`/api/favorites/${game_id}`, {
        method: 'POST',
    });

    const data = await favResponse.json();
    loadFavGames()
}

onMounted(() => {
    loadFavGames()
})

</script>

<template>
    <v-container v-if="!empty" class="mt-6">
        <v-row>
            <v-col
                v-for="game in games"
                :key="game.name"
                cols="12"
                sm="6"
                md="4"
            >
                <GameCard
                    :name="game.name"
                    :desc="game.desc"
                    :img="game.img"
                    :route="game.route"
                    :enabled="game.enabled"
                    :favorite="game.favorite"
                    @favorite-game="favoriteGame(game.id)"
                ></GameCard>
            </v-col>
        </v-row>
    </v-container>
    <v-container v-if="empty" class="mt-6">
        <v-card border="sm" color="grey-darken-3" class="quantico-bold">
            <v-carousel
                cycle
                crossfade
                :show-arrows="false"
                hide-delimiters
                :interval="3000"
                height="600"
            >
                <v-carousel-item
                    v-for="img in gameImgs"
                    :src="img"
                    :key="img">
                </v-carousel-item>
            </v-carousel>
            <v-card-title>You have no favorites...</v-card-title>
            <v-card-text>Let's go find some!</v-card-text>
            <v-btn class="ml-5 mb-5" href="#/games"> <!-- 1 = enabled -->
                <v-icon start>mdi-play-circle-outline</v-icon>
                Play some games!
            </v-btn>
        </v-card>
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