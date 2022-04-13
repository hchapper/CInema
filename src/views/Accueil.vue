<template>
    <div class="accueil">
        <h1>Recherche de films PWA</h1>
        <h2>Rechercher et critiquer des films</h2>
    </div>
    <h2><a href="https://www.ugc.fr">Liens vers UGC</a></h2>
    <h2> Les 20 films les plus populaires </h2>
        <table>
            <tr v-for="unFilm in populaire" v-bind:key="unFilm.key">
                <td class="titre1">{{unFilm.title}}</td>
                <td v-if="unFilm.poster_path!=null" >
                    <img class="poster"  v-bind:src="'http://image.tmdb.org/t/p/w500' + unFilm.poster_path"></td>
                <td>
                    <button class="bouton" v-on:click="voirDetails(unFilm)">Détails</button>
                </td>
            </tr>
        </table>
</template>



<script>
import axios from 'axios'
export default{
    name: 'Populaire',
    data ()  {
        return {
            baseUrl: 'https://api.themoviedb.org/3',
            api_key: '9021264f48ddf90a2c1bf9c58af5d837',
            films: [],
            populaire: [],
            }
    },

    methods: {
        async getpopulaire() {
            const response = await axios.get(this.baseUrl + '/movie/top_rated/?api_key=' + this.api_key);
            this.populaire = await response.data.results; 
        },

        voirDetails(film) {
                this.$router.push({
                    path: 'Details', query: {
                        id: film.id
                    }
                    
                });
            },
    },
    created() {
         this.film = {
            id: this.$route.query.id,
        }
        this.getpopulaire();
    }
}
</script>
