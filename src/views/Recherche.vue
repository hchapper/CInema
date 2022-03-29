<template>
    <div className="recherche">
        <form v-on:submit.prevent="recherche()">
            <div className="input-grouo">

                <label>Recherche</label>
                <input type="text" v-model="uneRecherche">

                <button class="bouton2" v-on:click="recherche">search</button>
            </div>
        </form>
        <h2> Résultats de la recherche </h2>
        <table>
            <tr v-for="unFilm in films" v-bind:key="unFilm.key">
                <td class="titre1">{{unFilm.title}}</td>
                <td v-if="unFilm.poster_path!=null" >
                    <img class="poster"  v-bind:src="'http://image.tmdb.org/t/p/w500' + unFilm.poster_path"></td>
                <td>
                    <button class="bouton" v-on:click="voirDetails(unFilm)">Détails</button>
                </td>
            </tr>
        </table>
    </div>    
</template>

<script>
import axios from 'axios'

export default{
    name: 'Recherche',
    data() {
        return {
            baseUrl: 'https://api.themoviedb.org/3',
            films: [],

            uneRecherche:'',
            api_key: '9021264f48ddf90a2c1bf9c58af5d837'
        }
    },
    methods: {
        async recherche() {
            //const response =  await axios.get(this.bseUrl + '/search.json?title=${this.query}');
            const response = await axios.get(this.baseUrl + '/search/movie?language=fr&api_key=' + this.api_key + '&query=' + this.uneRecherche);
            this.films = await response.data.results;
        },

        voirDetails(film) {
            this.$router.push({
                path: 'Details', query: {
                    id: film.id
                }
                
            });
        
        },
    }
}
</script>
