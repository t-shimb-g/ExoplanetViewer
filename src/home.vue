<script setup>
const cardInfo = [
    {
        title: "Explore",
        desc: "Click here to start exploring",
        image: "placeholder2.png"
    },
    {
        title: "Planet of the Day",
        desc: "Check out today's planet",
        image: "placeholder3.png"
    },
    {
        title: "Card 3",
        desc: "Card 3 description, cool stuff here!",
        image: "placeholder1.png"
    }
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
                    <v-img :src="item.image" height="300" cover/>
                    <v-card-title>{{ item.title }}</v-card-title>
                    <v-card-subtitle>{{ item.desc }}</v-card-subtitle>
                    <v-card-text>{{ item.description }}</v-card-text>
                    <v-btn class="ml-5 mb-5">
                        <v-icon start>mdi-space-station</v-icon>
                        Visit
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