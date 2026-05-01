<script setup>
const props = defineProps({
    name: { type: String, required: true },
    desc: { type: String, required: true },
    img: { type: String, required: true },
    route: { type: String, required: true },
    enabled: { type: Number, default: false, required: true }, // 0 or 1
    favorite: { type: Number, default: false, required: true }, // TODO: finish favorite-ing games
})

const emit = defineEmits(['favorite-game']);
</script>

<template>
    <v-card border="sm" color="grey-darken-3" class="quantico-bold">
        <v-img v-if="img" :src="img" height="500px" cover/>
        <v-card-title>{{ name }}</v-card-title>
        <v-card-text>{{ desc }}</v-card-text>
        <v-btn class="ml-5 mb-5" :href="'#/games/' + route" :disabled="enabled === 0"> <!-- 1 = enabled -->
            <v-icon start>mdi-play-circle-outline</v-icon>
            Play!
        </v-btn>
        <v-btn class="mr-5 mb-5 left" @click="emit('favorite-game')">
            <v-icon start>mdi-heart-outline</v-icon>
        </v-btn>
    </v-card>
</template>