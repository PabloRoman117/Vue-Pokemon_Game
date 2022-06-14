<template>

    <h1 v-if="!pokemon">Espere...</h1>
    <div v-else class="containter">
        <h1 class="bg-white text-warning p-4 rounded-pill shadow-lg mb-5">¿Quién es este pokémon?</h1>
        <PokemonPicture
         :pokemonId="pokemon.id" 
         :showPokemon="showPokemon"/>

        <PokemonOptions 
        :pokemons="pokemonArr" 
        @selection="checkAnswer()"/>

        <template v-if="showAnswer">
            <h2 class="fade-in">{{ message }}</h2>
            <button class="btn btn-primary" @click="newGame">
                Nuevo Juego
            </button>
        </template>
    </div>



</template>

<script>
import PokemonPicture from '@/components/PokemonPicture.vue'
import PokemonOptions from '@/components/PokemonOptions.vue'
import getPokemonOptions from '@/helpers/getPokemonOptions'

// console.log(getPokemonOptions())
export default {
    components: {
        PokemonPicture,
        PokemonOptions
    },

    data(){
        return {
            pokemonArr: [],
            pokemon: null,
            showPokemon: false,
            showAnswer: false,
            message: ''
        }
    },

    methods: {
         async mixPokemonArray(){
             this.pokemonArr = await getPokemonOptions()
             const rndInt = Math.floor(Math.random() * 4)
             this.pokemon = this.pokemonArr[rndInt]
        },

        checkAnswer( selectedId ) {
            this.showPokemon = true
            this.showAnswer  = true

            if( selectedId === this.pokemon.id ) {
                this.message = `Correcto, ${ this.pokemon.name }`
            } else {
                this.message = `Oops, era ${ this.pokemon.name }`
            }
        },
        newGame() {

            this.showPokemon = false
            this.showAnswer  = false
            this.pokemonArr  = []
            this.pokemon     = null
            this.mixPokemonArray()            

        }
    },

    mounted(){
        this.mixPokemonArray()
    }
}
</script>

