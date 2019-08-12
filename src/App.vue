<template>
  <div id="app">
    <div class="c-search-container">
      <!-- search form -->
      <form class="c-search"v-on:submit.prevent="fetchArtists">
      <input class="c-search__input" @keyup="fetchArtists" type="text" v-model="query" placeholder="Type Artist ..." >
      </form>
    </div>
    <div class="c-menu__container">
      <div class="row">
          <div v-for="item in menu" @click="type = item">{{item}}s</div>
      </div>
    </div>
          <!-- show the artists -->
    <div class="c-artist__container">
          
      <div class="c-artist-item" v-for="(artist, index) in artists" :key="index">
            
        <div  v-if="artist.images">
          
            <img v-if="artist.images[0]" :src="artist.images[0].url" width="100" />
              
          
        </div>
            <h1 class="c-artist__title">{{artist.name}}</h1>  
      </div>
    </div>
  </div>
</template>
<script>
const accessToken = "BQCv9qOJi_55AT4_lcrnO2ssV_3GiDTLhsBleB_O32VgIEXF5Z3QfswgJKx7EuHzsS2tQPomB_9Qw5zPQFovMklerln9qJcOi_i-LmjdZpdGd7T-9b7PkRUotvg-d-DTM29nZh9Hp4NqFkj-jHx9e3fo9pcOc-1_Hw"


const headers = {
  "Content-Type": "application/json",
  Authorization: `Bearer ${accessToken}`
};

  
  export default {
    name: 'app',
    data(){
      return {
        query: '',
        type: 'artist',
        artists: [],
        menu: ['artist', 'album', 'playlist','track']
      };
    },
    methods: {
      fetchArtists(name){
        let url = `https://api.spotify.com/v1/search?q=${this.query}&type=${this.type}`
        fetch(url, {headers})
          .then(res => res.json())
          .then(data => {
            console.log(data)
            this.artists = data[this.type+'s'].items
          })
      }
    },
    mounted(){
      this.fetchArtists();
    },
    components: {
      
    },
    watch: {
      type: function(){
        this.fetchArtists();
      }
    }
  }
</script>



<style>
#app {
  font-family: 'Avenir', Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #b3b3b3;
  margin-top: 60px;
  min-height: 100%;
  min-width: 720px;
}
*{margin:0;padding:0;-webkit-box-sizing:border-box;box-sizing:border-box}
body{
  min-height:100%;
  line-height:1.5;
  font-sizing:.875rem;
  font-family:Arial,Helvetica,sans-serif;
  background: #b3b3b3;
  min-width: 720px;
}

.c-search-container {
  position: absolute;
  z-index: 1;
  top: 0;
  left: 0;
  width: 100%;
}

.c-search {
  display: flex;
  justify-content: space-around;
  padding: 20px;
  min-width: 720px;
  background: rgba(0, 0, 0, .6);
}

.c-search__input {
  border: none;
  outline: none;
  color: #fff;
  font-size: 30px;
  background: transparent;
  text-transform: capitalize;
  min-width: 1240px;
  font-family: Arial;
  font-weight: bold;
  margin: 0 auto;
  position: sticky;

  
}
.c-artist-item {
  display: flex;
  align-items: center;
  cursor: pointer;
  padding: 10px 25px;
  border-bottom: 1px solid #333;
  background-color: #222;
  transition: all ease .3s;
  max-height: 400px;
  }

.row > div {
 background-color: #222;
  margin: 10px;
  padding: 5px 10px;
  font-size: 14px;
  font-weight: bold;
  font-family: arial;
  text-transform: uppercase;
  color: #b3b3b3;
  cursor: pointer;
  }

.row > div:hover {
  color: green;

}

.c-menu__container {
  display: flex;
  flex-direction: column;
  background-color: #222;
  
}
.c-menu__container > div {
  margin: 10px;
  padding: 20px;
  justify-content: center;
  color: #fff;
  transform: uppercase;
  font-weight: bold;
  display: flex;
}
.c-artist__container {
  display: block;
  height: 100%;
  background-color: #b3b3b3;
}


</style>
