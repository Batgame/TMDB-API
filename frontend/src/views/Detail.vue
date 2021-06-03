<template>
    <div class="container">
        <div class="detail">
            <div class="poster">
                <img 
                    :src="'https://image.tmdb.org/t/p/w500' + filmInfo.poster_path"
                    class="posterImage"
                    alt="Poster Image"
                />
            </div>
            <div class="data">
                <router-link to="/"><i class="far fa-times-circle fa-2x" style="float:right;"></i></router-link>
                <div class="title">
                    <h1>{{ filmInfo.title }}</h1>
                    <div class="genres">
                        {{ filmInfo.release_date }}
                        <span v-for="genre in filmInfo.genres" :key="genre.id" class="genre">{{ genre.name }}</span>
                    </div>
                </div>
                <div class="overview">
                    <h3 style="text-align: left;">Synopsys</h3>
                    <p>{{ filmInfo.overview }}</p>
                </div>
                <div class="whereToWatch" v-if="whereToWatch">
                    <div class="streaming">
                        <span>Voir sur: </span>
                        <img v-if="whereToWatch.flatrate" :src="'https://image.tmdb.org/t/p/w500' + whereToWatch.flatrate[0].logo_path" alt="logo" width="40">
                        <img v-if="whereToWatch.rent" :src="'https://image.tmdb.org/t/p/w500' + whereToWatch.rent[0].logo_path" alt="logo" width="40">
                    </div>
                </div>
            </div>
        </div>
        <div class="extras">
            <h3 style="text-align: left;">Casting</h3>
            <div class="cast">
                <div class="acteur" v-for="acteur in credits.slice(0, 10)" :key="acteur.id">
                    <div class="acteurImage">
                        <a :href="'https://www.themoviedb.org/person/' + acteur.id" target="_blank">
                            <img :src="'https://image.tmdb.org/t/p/original' + acteur.profile_path " alt="Photo acteur">
                        </a>
                    </div>
                    <div class="acteurText">
                        <p style="font-size: 0.9em;"><b>{{ acteur.name }}</b></p>
                        <p style="height: 40px; font-size: 0.8em;">{{ acteur.character }}</p>
                    </div>
                </div>
            </div>
            <div class="infos">
                <h3>Infos Clé</h3>
                <ul>
                    <li>Budget: </li>
                    <li>Recettes: </li>
                    <li>Langues: </li>
                    <li>Status: </li>
                </ul>
            </div>
        </div>
    </div>
</template>

<script>
import axios from 'axios'
export default {
    data: function(){
        return {
            filmInfo: [],
            whereToWatch: [],
            credits: []
        };
    },
    methods: {
        getInfo () {
            axios
            .get("https://api.themoviedb.org/3/movie/"+ this.$route.params.id +"?api_key=a509cff919b82d4a2be6b8f3b999a90f&language=fr-FR")
            .then((reponse) => this.filmInfo = reponse.data)
            .catch(error => console.log(error));
        },
        getWatcher() {
            axios
            .get("https://api.themoviedb.org/3/movie/"+ this.$route.params.id +"/watch/providers?api_key=a509cff919b82d4a2be6b8f3b999a90f")
            .then((reponse) => this.whereToWatch = reponse.data.results.FR)
            .catch(error => console.log(error));
        },
        getCredit() {
            axios
            .get("https://api.themoviedb.org/3/movie/"+ this.$route.params.id +"/credits?api_key=a509cff919b82d4a2be6b8f3b999a90f")
            .then((reponse) => this.credits = reponse.data.cast)
            .catch(error => console.log(error));
        }
    },
    beforeMount(){
        this.getInfo();
        this.getWatcher();
        this.getCredit();
    }
}
</script>

<style scoped>
h3
{
    margin: 0;
}
.container
{
    width: 100%;
    height: 700px;
    margin-top: 2%;
}

.title
{
    text-align: left;
}

.detail
{
    height: 65%;
    width: 90%;
    border-radius: 4px;
    background-color: white;
    padding: 5px;
    margin-left: 5%;
    margin-right: 5%;
    border: solid 1px #e3e3e3;
    border-bottom: none;
}
.posterImage
{
    border-radius: 10px;
    width: 250px;
    height: 375px;

}

.poster
{
    width: 30%;
    float: left;
    height: 100%;
    text-align: center;
}
.data
{
    float: right;
    width: 65%;
    height: 80%;
}

.genre
{
    background-color: #e8e8e8;
    padding: 3px;
    margin: 3px;
    border-radius: 4px;
    display: inline-block;
}

.extras
{
    height: 600px;
    width: 95%;
    padding: 5px;
    margin-left: 2.5%;
    margin-right: 2.5%;

}
.whereToWatch
{
    margin-top: 2%;
}

.whereToWatch img
{
    margin-left: 1%; 
    border-radius: 4px;
}
a
{
    color: inherit;
    text-decoration: none;
}

.cast
{
    width: 70%;
    float: left;
    height: 295px;
    overflow: auto;
    white-space: nowrap;
}
.acteur
{
    height: 250px;
    width: 138px;
    display: inline-block;
    margin: 1%;
    border-radius: 4px;
    border: 1px solid #e3e3e3;
    box-shadow: 0 2px 8px rgb(0 0 0 / 10%);
}

.acteur p
{
    margin: 0;
    padding: 0 10px;
}

.acteurImage
{
    height: 175px;
    width: 100%;
}

.acteurImage img
{
    width: 100%;
    height: 100%;
    border-radius: 5px;
}

.acteurText
{
    margin-top: 10px;
}

.infos
{
    float: right;
    width: 27%;
    height: 50%;
    background-color: rgb(179, 172, 82);
}

.streaming
{
    margin: 5% 0;
}
</style>