
<template>
<div id = app>
  
  <header>
    <h1> Music Player </h1>

  </header>
 <main>
   <section class = 'player'>
     <h2 class = 'title'> {{current.title}} - <span> {{current.artist}} </span></h2>
     <br>
     <div class = 'button'>
       <button class = 'prev' @click="prev" > Prev </button>
       <button class = 'play' v-if="!isPlaying" @click="play"> Play </button>
       <button class = 'pause' v-if="isPlaying" @click="pause"> Pause </button>
       <button class = 'next' @click="next">  Next  </button>
        <button class = 'shuffle' v-if="isShuffle" @click="not_shuffle">  Shuffle  </button>
        <button class = 'not_shuffle' v-if="!isShuffle" @click="shuffle">  Shuffle  </button>
     </div>
   </section>
   <section class = 'playlist'>
     <br>
     <h3>Music Queue</h3>
     <button v-for="song in songs" :key="song.src" @click="play(song)" :class="(song.src == current.src ? 'song playing' : 'song')">
          {{ song.title }} - {{ song.artist }}
        </button>
   </section>

 </main>
</div>
 
</template>

<script>
export default {
  name: 'app',
  data (){
    return{
      default_songs : [
         {  
            id : 0,
            title: 'Careless',
            artist: 'Neffex',
            src: require('./assets/NEFFEX - Careless .mp3')
          },
           {
              id : 1,
            title: 'Fight Back',
            artist: 'Neffex',
            src: require('./assets/NEFFEX - Fight Back .mp3')
          },
           {
              id : 2,
            title: 'Stereo Hearts',
            artist: 'Gym Class Heroes',
            src: require('./assets/Gym Class Heroes Stereo Hearts ft. Adam Levine.mp3')
          },
           {
              id : 3,
            title: 'Lips of an Angel',
            artist: 'Hinder',
            src: require('./assets/Hinder - Lips Of An Angel.mp3')
          },
           {
              id : 4,
            title: 'Breakeven',
            artist: 'The Script',
            src: require('./assets/The Script - Breakeven .mp3')
          }
      ],
      index : 0,
      isPlaying : false,
      isShuffle : false,
      player : new Audio(),
      songs : null
    }
  },
  created(){
  this.songs = this.default_songs;

this.current = this.songs[this.index];
this.player.src = this.current.src;
},
methods :{
  play(song){
    this.pause();
     if( typeof song.src != 'undefined') {
          this.current = song;
           console.log(this.current.title);
          this.player.src = this.current.src;
        }

        this.player.play()
        this.isPlaying = true;
 
  },pause(){
        this.player.pause();
        this.isPlaying = false;
  },
  next(){
        this.index++;
        if(this.index > this.songs.length - 1) {
          this.index = 0;
        }

        this.current = this.songs[this.index];
        this.pause();
        this.play(this.current);
      },
      prev(){
          this.index--;
        if(this.index < 0) {
          
          this.index = this.songs.length-1;
          console.log(this.index);
        }
        this.pause();
        this.current = this.songs[this.index];
        this.play(this.current);
      },
      getRandom(){
        return Math.floor(Math.random()*5);
      },shuffle(){
       this.songs = [];
        var select = [];
        var new_queue = [];
        var count = 0;
        new_queue[0] = this.current;

        select[count] = this.current.id;
        
        count++;
        while(count < 5){
          
          var random = this.getRandom();
          var x = 0;
          var y = 0;
          while(x < 4){
           
            if(random == select[x]){
              y = 1;
            
              break;
            }
            x++;
          }
          if(y == 0){
            select[count] = random;
            new_queue[count] = this.default_songs[random];
            count++;
            
          }
        }
        this.isShuffle = true;
        this.songs = [];
        this.songs = new_queue;
        this.pause();
         this.play(this.current);
        
      },not_shuffle(){

        this.songs = [];
        this.songs = this.default_songs;
        
        this.isShuffle = false;
        this.pause();
         this.play(this.current);
        
      }
  
}
 }
</script>

<style>
*{
    margin: 0;
    padding: 0;
    box-sizing: border-box;
  }
  body{
    font-family: 'Times New Roman';
     
     background-image: url('./assets/bg.jpg');
    background-size: cover;
  }
  header {
    display: flex;
    justify-content: center;
    align-items: center;
    padding: 15px;
    background-color: rgb(0, 0, 0);
    color: rgb(255, 255, 255);
    font-style: normal;
  }

  
  main {
    width: 100%;
    max-width: 768px;
    margin: 0 auto;
    padding: 25px;
    text-align: center;
  }

  .song-title {
    color: #53565a;
    font-size: 32px;
    font-weight: 700;
    text-transform: uppercase;
    text-align: center;
  }

  .song-title span {
    font-weight: 400;
    font-style: italic;
  }

  .controls {
    display: flex;
    justify-content: center;
    align-items: center;
    padding: 30px 15px;
  }

  button {
    appearance: none;
    background: none;
    border: none;
    outline: none;
    cursor: pointer;
  }

  button:hover {
    opacity: 0.8;
  }

  .play, .pause {
    font-size: 20px;
    font-weight: 700;
    padding: 15px 25px;
    margin: 0px 15px;
    border-radius: 8px;
    color: #fff;
    background-color: #06fa2e;
  }

  .next, .prev,.shuffle {
    font-size: 16px;
    font-weight: 700;
    padding: 10px 20px;
    margin: 0px 15px;
    border-radius: 6px;
    color: #fff;
    background-color: #419746;
  }
  .not_shuffle {
    font-size: 16px;
    font-weight: 700;
    padding: 10px 20px;
    margin: 0px 15px;
    border-radius: 6px;
    color: #fff;
    background-color: #f58c8c;
  }

  .playlist {
    padding: 0px 30px;
   
  }

  .playlist h3 {
    color: #212121;
    font-size: 28px;
    font-weight: 400;
    margin-bottom: 30px;
    text-align: center;
  }

  .playlist .song {
    display: block;
    width: 100%;
    padding: 15px;
    font-size: 20px;
    font-weight: 700;
    cursor: pointer;
  }

  .playlist .song:hover {
    color: #58ff6e; 
  }

  .playlist .song.playing {
    color: #fff;
    background-image: linear-gradient(to right, #53cc2e, #95ff58);

  }
</style>
