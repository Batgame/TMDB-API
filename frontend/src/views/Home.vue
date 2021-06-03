<template>
  <div id="app">
    <div class="search">
      <select v-model="cat" id="cat" @change="getData">
        <option value="popular">Popular</option>
        <option value="now_playing">Now Playing</option>
        <option value="upcoming">Upcoming</option>
        <option value="top_rated">Top Rated</option>
      </select>
      |
      <input type="text" v-model="search" placeholder="Ant-Man, X-Men..." @keyup="searchMovie">
    </div>
    <div class="container">
      <div v-for="film in films" class="film" :id="film.id" :key="film.id">
        <router-link :to="'movie/' + film.id">
          <img v-if="film.poster_path != null"
            :src="'https://image.tmdb.org/t/p/w500' + film.poster_path"
            class="poster"
            alt="Poster Image"
          />
          <img v-else
            src="http://www.booooooom.com/wp-content/uploads/2015/04/emptyfilmposters-08.jpg"
            class="poster"
            alt="Poster Image"
          />
        </router-link>
      </div>
    </div>
  </div>
</template>

<script>
const axios = require("axios");
export default {
  name: "HelloWorld",
  props: {
    msg: String,
  },
  data: function(){
    return { 
      title: "TMDB Content API",
      cat: "popular",
      search: "",
      films: []
    };
  },
  methods: {
    getData() {
      axios
        .get("https://api.themoviedb.org/3/movie/" + this.cat + "?api_key=a509cff919b82d4a2be6b8f3b999a90f&language=fr-FR&page=1")
        .then((reponse) => (this.films = reponse.data.results))
        .catch((error) => console.log(error));
      //console.log(this.cat);
      //console.log(this.films);
    },
    searchMovie() {
      axios
        .get("https://api.themoviedb.org/3/search/movie?api_key=a509cff919b82d4a2be6b8f3b999a90f&language=fr-FR&query="+ this.search +"&page=1")
        .then((reponse) => (this.films = reponse.data.results))
        .catch((error) => console.log(error));
    }
  },
  beforeMount(){
    this.getData();
  }
};
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style lang="scss">
input
{
  flex: 1 1 0%;
  padding: 10px 13px 12px;
  border-radius: 4px;
  appearance: none;
  border: none;
  font-family: Graphik, sans-serif;
  font-size: 16px;
  font-weight: normal;
  line-height: 1.75;
  color: rgb(11, 52, 102);
  background-color: white;
}
select
{
  flex: 1 1 0%;
  padding: 10px 13px 12px;
  border-radius: 4px;
  appearance: none;
  border: none;
  font-family: Graphik, sans-serif;
  font-size: 16px;
  font-weight: normal;
  line-height: 1.75;
  color: rgb(11, 52, 102);
  background-color: white;
}
.container {
  display: inline-block;
  width: 100%;
  text-align: center;
}

.film {
  width: 250px;
  height: 375px;
  display: inline-block;
  margin: 1%;
}

.poster {
  width: 100%;
  height: 100%;
}

img {
  border-radius: 10px;
}

.streaming
{
  text-align: left;
  display: flex;
  align-items: center;
}
</style>
