<template>
      <div class="cards">
    <card 
    class="card"
     v-for="pokemon in pokemons" 
     :key="pokemon"
     @click="click(pokemon)"
     :class="{opace: selectedId && pokemon.id !== selectedId}"
     classe="card">
        <template v-slot:title>
            {{ pokemon.name }} # {{ pokemon.id }}
        </template>    
        <template v-slot:content>
            <img :src="pokemon.sprite"/>
        </template>
        <template v-slot:description>
            <div v-for="type in pokemon.types" :key="type">
            {{ type }}
            </div>
        </template>
    </card>
    </div>    
</template>
<script>
import Card from './Card.vue'

export default {
    components: {
        Card
    },
    props: {
        pokemons: {
            type: Array,
            mendatory: true
        },
        selectedId: {
            type: Number
        }
    },
    methods: {
        click(pokemon) {
            this.$emit("pokemonClicked", pokemon)
        }
    }    
}
</script>
<style scoped>
.cards {
    display: flex;
}
 .opace {
     opacity: 0.5;
 }
 .card:hover {
     opacity: 1.0;
 }
 img {
     width: 100%;
 }
</style>