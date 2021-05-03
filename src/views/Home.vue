<template>
  <div class="home">
    <div class="featured-card">
      <router-link to="/movie/tt0409591">
        <img
          src="https://assets.puzzlefactory.pl/puzzle/186/022/original.jpg"
          alt="Naruto poster"
          class="featured-img"
        />
        <div class="detail">
          <h3>Naruto</h3>
          <p>
            Naruto es un niño huérfano,que fue odiado desde pequeño por llevar
            adentro al Kyubi , al crecer se vuelve un ninja de konoha y es
            integrante del equipo 7 ,con en tiempo se vuelve el mejor ninja y al
            fin es amado por todos y se vuelve hokage
          </p>
        </div>
      </router-link>
    </div>

    <form @submit.prevent="searchMovie()" class="search-box">
      <input type="text" placeholder="¿qué quieres ver?" v-model="search">
      <button type="submit">Buscar</button>
    </form>

    <div class="movies-list">
      <div class="movie" v-for="movie in movies" :key="movie.imdbID">
        <router-link :to="'/movie/' + movie.imdbID" class="movie-link">
          <div class="product-image">
            <img :src="movie.Poster" alt="Movie poster">
            <div class="type">{{ movie.Type }}</div>
          </div>
          <div class="detail">
            <p class="year">{{ movie.Year }}</p>
            <h3>{{ movie.Title }}</h3>
          </div>
        </router-link>  
      </div>
    </div>
  </div>
</template>

<script>
import { ref } from 'vue'
import env from '@/env.js'
export default {
  name: "Home",
  // setup() {
  //   const search = ref("")
  //   const movies = ref([])

  //   const searchMovie = () => {
  //     if(search.value != ""){
  //       console.log(search.value);
  //     }
  //   }

  //   return{
  //     search,
  //     movies,
  //     searchMovie
  //   }
  // }
  data() {
    return {
      search:'',
      movies:[]
    }
  },
  methods: {
    searchMovie(){
      if(this.search != ""){
        fetch(`http://www.omdbapi.com/?apikey=${env.apikey}&s=${this.search}`)
        .then(response => response.json())
        .then(data => {
          this.movies = data.Search
          console.log(this.movies);
        })
        this.search = ''
      }
    }
  },
};
</script>
<style lang="scss">
.home{
  .featured-card{
    position: relative;

    .featured-img{
      display: block;
      width: 100%;
      height: 300px;
      object-fit: cover;
      position: relative;
      z-index: 0;
    }

    .detail{
      position: absolute;
      left: 0;
      right: 0;
      bottom: 0;
      background-color: rgba(0, 0, 0, 0.6);
      padding: 16px;

      h3{
        color: #FFF;
        margin-bottom: 16px;
      }

      p{
        color: #FFF;
      }
    }
  }
  .search-box{
    display: flex;
    flex-direction: column;
    justify-content: center;
    align-items: center;
    padding: 16px;

    input{
      display: block;
      appearance: none;
      border: none;
      outline: none;
      background: none;

      &[type="text"] {
        width: 100%;
        color: #FFF;
        background-color: #496583;
        font-size: 20px;padding: 10px 16px;
        border-radius: 8px;
        margin-bottom: 15px;
        transition: 0.4s;

        &::placeholder{
          color: #f3f3f3;
        }

        &:focus{
          box-shadow: 0px 3px 6px rgba(0, 0, 0, 0.2);
        }
      }
    }
    button{
      width: 100%;
      max-width: 300px;
      background-color: #42B883;
      padding: 16px;
      border-radius: 8px;
      color: #FFF;
      font-size: 20px;
      text-transform: uppercase;
      transition: 0.4s;
      border: none;

      &:active{
        background-color: #3B8070;
      }
    }
  }
  .movies-list{
    display: flex;
    flex-wrap: wrap;
    margin: 0px 8px;

    .movie{
      flex: 1 1 50%;
      padding: 16px 8px;

      .movie-link{
        display: flex;
        flex-direction: column;
        height: 100%;

        .product-image{
          position: relative;
          display: block;

          img{
            display: block;
            width: 100%;
            height: 275px;
            object-fit: cover;
          }
          .type{
            position: absolute;
            padding: 8px 16px;
            background-color: #42B883;
            color: #FFF;
            bottom: 16px;
            left: 0px;
            text-transform: capitalize;
          }
        }
        .detail{
          background-color: #496583;
          padding: 16px 8px;
          flex: 1 1 100%;
          border-radius: 0px 0px 8px 8px;

          .year{
            color: #AAA;
            font-size: 14px;
          }
          h3{
            color: #FFF;
            font-weight: 600;
            font-size: 18px;
          }
        }
      }


    }
  }
}
</style>
