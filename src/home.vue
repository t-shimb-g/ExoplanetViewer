<script setup>
import {onMounted, ref} from "vue";
import GameCard from "@/components/gameCard.vue";

const gameImgs = ref([])
const favGameImgs = ref([])

const loading = ref(true)
const error = ref('')

async function loadGames() {
    loading.value = true;
    error.value = '';

    try {
        const response = await fetch('/api/games') // TODO: Change back to /api/games
        if (!response.ok) {
            throw new Error('Failed to load games');
        }
        const data = await response.json();
        gameImgs.value = data.map(game => game.img)
        console.log(gameImgs.value);

        const favResponse = await fetch('/api/favorites')
        if (!favResponse.ok) {
            throw new Error('Failed to load games');
        }
        const favData = await favResponse.json();
        favGameImgs.value = favData.map(favGame => favGame.img)
        favGameImgs.value = favGameImgs.value.sort(() => Math.random() - 0.5);
        console.log(favGameImgs.value);

    } catch (err) {
        error.value = err.message || 'Something went wrong while loading games'
    } finally {
        loading.value = false;
    }
}

onMounted(() => {
    loadGames()
})

</script>

<template>
    <v-container class="mt-6">
        <v-row>
            <v-col
                cols="6"
            >
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
                    <v-card-title>Board Games</v-card-title>
                    <v-card-text>Jump into these classic board games</v-card-text>
                    <v-btn class="ml-5 mb-5" href="#/games"> <!-- 1 = enabled -->
                        <v-icon start>mdi-play-circle-outline</v-icon>
                        Play some games!
                    </v-btn>
                </v-card>
            </v-col>
            <v-col
                cols="6"
            >
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
                            :show-arrows="false"
                            v-for="img in favGameImgs"
                            :src="img"
                            :key="img">
                        </v-carousel-item>
                    </v-carousel>
                    <v-card-title>Favorites</v-card-title>
                    <v-card-text>Go back to what you love</v-card-text>
                    <v-btn class="ml-5 mb-5" href="#/favorite-games"> <!-- 1 = enabled -->
                        <v-icon start>mdi-cards-playing-heart-multiple</v-icon>
                        Manage your favorites!
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