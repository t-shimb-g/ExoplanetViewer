<script setup>
import {onMounted, ref} from "vue";
import GameCard from "@/components/gameCard.vue";

const games = ref([])
const loading = ref(true)
const error = ref('')

async function loadGames() {
    loading.value = true;
    error.value = '';

    try {
        const response = await fetch('/api/games')

        if (!response.ok) {
            throw new Error('Failed to load games');
        }

        const data = await response.json();

        games.value = data.map((game) => ({
            id: game.id,
            name: game.name,
            desc: game.desc,
            rules: game.rules,
            img: game.img,
            route: game.route,
            enabled: game.enabled
        }))

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
                ></GameCard>
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