<template>
    <h1 v-if="!pokemon">Espere por favor...</h1>
    <div v-else>
        <h1>¿Quién es ese pokemon?</h1>
        <PokemonPicture :pokemon-id="pokemon.id" :pokemon-show="showPokemon"/>
        <PokemonOptions :pokemons="pokemonArr" @selection="checkAnswer"/>
    </div>
    <div v-if="showAnswer">
        <h2>{{message}}</h2>
        <button @click="newGame">Nuevo juego</button>
    </div>
</template>

<script>
import PokemonOptions from '../components/PokemonOptions'
import PokemonPicture from '../components/PokemonPicture'
import getPokemonOptions from '../helpers/getPokemonOptions'

export default {
    components: { PokemonPicture, PokemonOptions },
    data() {
        return {
            pokemonArr: [],
            pokemon: null,
            showPokemon: false,
            showAnswer: false,
            message: ''
        }
    },
    methods: {
        async mixPokemonArray() {
            this.pokemonArr = await getPokemonOptions()
            const rndInt = Math.floor(Math.random() * 4)
            this.pokemon = this.pokemonArr[rndInt]
        },
        checkAnswer(pokemonId) {
            this.showPokemon = true
            this.showAnswer = true
            if (pokemonId === this.pokemon.id)
                this.message = `Correcto! Es ${this.pokemon.name}`
            else {
                this.message = `Oops! Es ${this.pokemon.name}`
            }
        },
        newGame() {
            this.pokemonArr = [],
            this.pokemon = null,
            this.showPokemon = false,
            this.showAnswer = false,
            this.message = '',
            this.mixPokemonArray()
            
        }
    },
    mounted() {
        this.mixPokemonArray()
    }
}
</script>