<template>
    <link href="Style.css" rel="stylesheet">
    <div class="details">
        <div class="content">

            <div class="settings-head">
				<div>
					<table>
						<tr>
							<td v-if="details.poster_path">
                                <img class="icone" v-bind:src="'http://image.tmdb.org/t/p/w500' + details.poster_path">
                            </td>
							<td>
								<h2>{{details.title}}</h2>
                                <p>{{details.overview}}</p>
                            </td>
						</tr>
                    </table>
				</div>
			</div>

            <h2>Note des utilisateurs</h2>
            <div class="vote">
                
                <div class="vote_bck" v-bind:style="styles"></div>
            </div>
            <!-- <img src="img/stars.png"> -->
            <b>{{(details.vote_average /2)}} / 5</b> 
                <br>
            <i>Basé sur le vote de {{details.vote_count}} personnes.</i>
                <br><br>
            <div class="hr"></div>

             <div v-if="voted" class="back" style="margin: -40px 15px 15px 15px; float: right; text-align: center">
                <b>Merci !</b>
            </div>
           <h3>Quelle note donnez vous au film ?</h3>
            <div v-if="!voted" class="back" style="margin: -40px 15px 15px 15px; float: right;  text-align: center">
                <h3>Quelle note donnez vous au film ?</h3>
                <span v-for="n in 5" v-bind:key="n">
                    <img  v-if="n > note" class="note" src="public\img\icons\star.png"   style="filter: grayscale(100%);"    v-on:click="setnote(n)">
                    <img  v-if="n <= note" class="note" src="public\img\icons\star.png"      v-on:click="setnote(n)">                    
                </span>
                    <br>
                <button v-on:click="postreview">Noter</button>
            </div>

            <h2>Casting</h2>
            <table class="cast" v-for='credit in credits.cast' v-bind:key="credit.key">
                <tr>
                    <td v-if="credit.profile_path"><img class="profile" v-bind:src="'http://image.tmdb.org/t/p/w500' + credit.profile_path"></td>
                    <td>
                        <b>{{ credit.name }}</b><br>
                        {{ credit.character }}
                    </td>
                </tr>
            </table>
            
            <div class="back">
                <h2>Crew</h2>
                <table class="cast" v-for='credit in credits.crew' v-bind:key="credit.key">
                    <tr>
                        <td>
                            <b>{{ credit.name }}</b>
                        </td>
                    </tr>
                </table>
            </div>

            <div>
    <h4> Films Recommandés </h4>
    <div v-for="recommendation in recommendations.results" v-bind:key="recommendation.key">
        {{recommendation.title}}
    </div>
    </div>

        </div>
    </div>
    
</template>

<script>
import axios from 'axios'
export default{
    name: 'Details',
    data: () => ({
        baseUrl: 'https://api.themoviedb.org/3',
        api_key: '9021264f48ddf90a2c1bf9c58af5d837',
        film: [],
        details: [],
        credits: [],
        reviews: [],
        recommendations : [],
        load: true,
    }),
    methods: {
        async getdetails() {
            const response = await axios.get(this.baseUrl + '/movie/' + this.film.id + '?language=fr&api_key=' + this.api_key);
            this.details = await response.data; 
        },
        async getcredits() {
            const response = await axios.get(this.baseUrl + '/movie/' + this.film.id + '/credits?language=fr&api_key=' + this.api_key);
            this.credits = await response.data;  
        },
        async getreviews() {
            const response = await axios.get(this.baseUrl + '/movie/' + this.film.id + '/reviews?language=fr&api_key=' + this.api_key);
            this.reviews = await response.data;  
        },
        async getrecommendations(){
            const response = await axios.get(this.baseUrl + '/movie/' + this.film.id + '/recommendations?api_key=' + this.api_key +'&language=fr' );
            this.recommendations = await response.data;
            //https://api.themoviedb.org/3/movie/{movie_id}/recommendations?api_key=<<api_key>>&language=en-US&page=1
        }
    },
    created() {
        this.film = {
            id: this.$route.query.id,
        }
        
        this.getdetails();
        this.getcredits();
        this.getrecommendations();
        this.load = false;
    },
    computed: {
        styles: function() {
            return {
                width: (this.details.vote_average * 10) + '%',
            };
        }
    }
}
  
</script>