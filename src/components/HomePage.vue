<template>
  <div class="root">
    <div class="menuBar">
      <center>
        <h1>Labrashow Movies</h1>
      </center>
    </div>

    <center>
    <div id="movieAPI" class="movieAPI">
      <p>Need help choosing a movie?</p>
      <p>Let us give you a recommendation!</p>

      <button v-on:click="query()">Randomize</button>

      <br/>

      <div id="results" class="results">
        <span v-html="output"></span>
      </div>

      <div id="remarks" class="remarks">
        <h2>ENJOY THE SHOW!</h2>
      </div>
    </div>
    </center>

    <div class="gitHubLink">
      <center>
      <a href="https://github.com/tcniedfeldt/Labrashow">My GitHub Repository</a>
      </center>
    </div>
  </div>
</template>

<script>
import axios from 'axios';

export default {
	name: 'HomePage',
	data() {
		return {
			output: '',
			posts: [],
			errors: []
		}
	},
	methods: {
		query: function(e) {
                	var movie = Math.floor(Math.random() * 1000) + 2;
                	var myurl="https://api.themoviedb.org/3/movie/" + movie + "?api_key=c59173680a889c0419dae6d8320bd867";

			axios.get(myurl)
			.then( response => {
				console.log(response);

                                var results = '';
                                results += '<div class="poster_image"><img src="https://image.tmdb.org/t/p/w154' + response.data.poster_path + '"/></div>';
                                results += '<div class="description"><h3>' + response.data.title + '</h3>';
                                results += '<p>' + response.data.tagline + '</p>';

                                //Genres
                                results += '<p>Genre(s): ';
                                for (var i = 0; i < response.data.genres.length; i++) {
                                        if (i === 0) {
                                                results += response.data.genres[i].name;
                                        } else {
                                                results += ', ' + response.data.genres[i].name;
                                        }
                                }
                                results += '</p></div>';

				//Output results
				console.log(results);

				document.getElementById('results').innerHTML = results;

				output = results;
			})
			.catch (e => {
				this.errors.push(e);
			})
		}
	}
}
</script>

<style scoped>
      body {
        background: #282828;
        color: #FFFFFF;
        font-family: "Impact", Charcoal, sans-serif;
      }

      .results {
        display: grid;
        grid-column-gap: 50px;
        grid-template-columns: auto auto;
        background-color: #800000;
        margin-bottom: 5%;
        margin-top: 5%;
        padding: 3%;
      }

      img {
        float:right;
      }

      .poster_image {
        grid-column-start: 1;
        grid-column-end: 2;
      }

      .description {
        grid-column-start: 2;
        grid-column-end: 3;
        text-align: left;
      }

      .gitHubLink {
        background-color: #800000;
        padding: 3%;
      }
</style>
