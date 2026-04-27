<script setup>
const props = defineProps({
    name: { type: String, required: true },
    desc: { type: String, required: true },
    img: { type: String, required: true }
})

</script>

<template>
    <v-card border="sm" color="grey-darken-3" class="quantico-bold">
        <v-img v-if="img" :src="img" height="500px" cover/>
        <v-card-title>{{ name }}</v-card-title>
        <v-card-text>{{ desc }}</v-card-text>
        <v-btn class="ml-5 mb-5">
            <v-icon start>mdi-play-circle-outline</v-icon>
            Play!
        </v-btn>
    </v-card>
</template>