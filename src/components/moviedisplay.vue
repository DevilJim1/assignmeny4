<script setup>
import axios from "axios";
import { ref } from "vue";

const moviename = ref(null);
const movieData = ref(false);
let trailers = ref([]);
let errorOccured=false;

const mvn = async () => {
  trailers = ref([]);
  const data = moviename.value;
  movieData.value = (
    await axios.get("https://api.themoviedb.org/3/search/movie", {
      params: {
        api_key: "7f54447bdb40934992e1a1ea74a43b9a",
        include_adult: "false",
        query: data,
      },
    })
  ).data.results[0];
  
  console.log(movieData.value);

trailers.value.push(
  (
    await axios.get(`https://api.themoviedb.org/3/movie/${movieData.id}`, {
      params: {
        api_key: "7f54447bdb40934992e1a1ea74a43b9a",
        append_to_response: "videos",
      },
    })
  ).data.videos.results
    .filter((trailer) => trailer.type === "Trailer")
    .at(0).key
);      
}
</script>

<template>
  <label id="Header">T M D B</label>

  <div class="moviecontainer">
   <div class="selector">
    <select v-model="moviename" id="movies">
      <optgroup label="Classics">
        <option value = "Forest Gump">Forest Gump</option>
        <option value = "The Shawshank Redemption">The Shawshank Redemption</option>
        <option value = "The Godfather part">The Godfather</option>
        <option value = "Hobbit: The Unexpected">The Hobbit</option>
        <option value = "One Flew Over the Cuckoo's Nest">One Flew Over the Cuckoo's Nest</option>
      </optgroup>
      <optgroup label="Horror Movies">
        <option value = "the shining">The Shining</option>
        <option value = "Ju-On: The Grudge 1">Ju-On: The Grudge</option>
      </optgroup>
      <optgroup label="Animation Movies">
        <option value = "Pirates of the Caribbean: The Curse of the Black Pearl">Pirates of the Caribbean 1</option>
        <option value = "Spirited Away">Spirited Away</option>
        <option value = "The Peanuts Movie">The Peanuts Movie</option>
        <option value = "The Lego Movie 1">The Lego Movie</option>
      </optgroup>
    </select>
    <button @click="mvn" class="button">Get</button>
  </div>
  <div class="wrapper">
    <div v-if="movieData" class="descript">
    <br/><br/>
      <p>-- {{ movieData.title }}--{{movieData.release_date}} </p>
      <p> {{ movieData.overview }} </p>
      <br/> <p>lan: {{movieData.original_language}} - Popularity: -{{movieData.popularity}} - Vote Average: -{{movieData.vote_average}}</p>
      <iframe class="trailer" v-if="movieData" :src="`https://www.youtube.com/embed/${trailers[index]}`" alt="there's no video available"/>
    </div>
    <div>
    <img
      v-if="movieData.poster_path"
      :src="'https://image.tmdb.org/t/p/w500' + movieData.poster_path"
      class="poster"
    />
    </div>
  </div>
  </div>
</template>

<style scoped>
.wrapper{
  display:grid;
  grid-template-columns: 60% 40%;
}

 #Header {
  text-align:center;
  font-size:100px;
  color:white;
  display:block;
 }
 
 .selector{
  display:block
 }
 .poster {
    float:top;
    float:right;
    width: 390px;
    height:auto;
    margin-right:200px;
  }
  
  .descript {
    float:left;
    color: black;
    font-weight: lighter;
    font-family: Arial, Helvetica, sans-serif;
    width:450px;
    height:260px;
  }
  
  .trailer {

    float:left;
    height: 300px;
    aspect-ratio: 16 / 9;
  }

  div{
    background-color:#888;
  }
  
  body {
    background-color:#888;
    margin-left:40px;
  }

  .button {
    display:inline;
  }

  #movies {
    display:inline;
  }

  #movies > optgroup{
    font-weight:bolder;
    color:darkred;
  }

  #movies > optgroup > option{
    color:black;
  }

</style>