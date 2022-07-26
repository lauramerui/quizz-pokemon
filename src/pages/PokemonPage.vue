<template>
    <h1 v-if="!pokemon">Espere por favor...</h1>
    <div v-else>
        <h1>¿Quién es este pokémon?</h1>
        <PokemonPicture :pokemon-id="pokemon.id" :show-pokemon="showPokemon" />
        <PokemonOptions :pokemons="pokemonArr" @selection-pokemon="checkAnswer" />
        <p v-if="puntos!=0">Aciertos: {{ puntos }}</p>
        <p v-else>Aún no has acertado ningún pokemon!</p>
        <div v-if="showAnswer" class="fade-in">
            <h2>{{ message }}</h2>
            <button @click="newGame">Siguiente</button>
        </div>
        
    </div>
</template>

<script>

import PokemonOptions from '@/components/PokemonOptions.vue'
import PokemonPicture from '@/components/PokemonPicture.vue'

import getPokemonOptions from '@/helpers/getPokemonOptions'

export default {
    components: {
        PokemonPicture,
        PokemonOptions
    },
    data() {
        return {
            pokemonArr: [],
            pokemon: null,
            showPokemon: false,
            showAnswer: false,
            message: '',
            puntos: 0
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
            this.message = this.pokemon.id === pokemonId ? `Correcto, ${this.pokemon.name}` : `Oops, era ${this.pokemon.name}`
            this.puntos = this.pokemon.id === pokemonId ? this.puntos + 1 : this.puntos
        },
        newGame() {
            this.showAnswer = false
            this.showPokemon = false
            this.pokemon = null
            // this.pokemonArr = []
            this.mixPokemonArray()
        }
    },
    mounted() {
        this.mixPokemonArray()
    }

}
</script>

