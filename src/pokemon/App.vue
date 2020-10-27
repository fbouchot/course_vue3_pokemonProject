
<template>
    <pokemon-cards
        :pokemons="starters"
        :selectedId="selectedId"
        @pokemonClicked="fetchEvolutions"/>
    
    <pokemon-cards :pokemons="pokemonEvolutions"/>    
</template>
<script>
import Card from './Card.vue'
import PokemonCards from './PokemonCards.vue'
const api = 'https://pokeapi.co/api/v2/pokemon';
const STARTER_IDS = [1, 4, 7];

export default {
    components: {
        Card,
        PokemonCards
    },
    data() {
        return {
           starters:[],
           pokemonEvolutions: [],
           selectedId: null
        }
    },
    async created() { //call whe the component is created in MEMORY 
        // console.log('created');
        // console.log(this.$el); //will be null on reload
        const starters = await this.fetchData(STARTER_IDS);
        this.starters = starters;
    },
    // mounted(){ //call whe the component appear in the DOM-->
    //     console.log('mounted');
    //     console.log(this.$el);
    // },
    methods: {
        async fetchEvolutions(pokemon) {
        this.selectedId = pokemon.id;
        const pokemonIds =[pokemon.id + 1, pokemon.id + 2];        
        const evolutions = await this.fetchData(pokemonIds);
        this.pokemonEvolutions = evolutions;
        },       
       async fetchData(ids) {
        
        // await for all 3 calls to be called async
        const responses = await Promise.all(ids.map(id => window.fetch(`${api}/${id}`)));
        
        //map as json the responses
        const data = await Promise.all(responses.map(res => res.json()));

        const result = data.map(d => ({
                id: d.id,
                name: d.name,
                sprite: d.sprites.other['official-artwork'].front_default,
                types: d.types.map(type => ( type.type.name))               
            }));        
         
         return result;
        }
    }
}
    
</script>

<style scoped>

</style>