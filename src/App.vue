<template>
  
  <div id="app" class="custom-cursor">
    <div class="container">
      <main>
    <header>
    <h1>Peak Musique</h1>
   </header>
    <section class="lecteur">
      <div class="image-musique"><img src="" id="image-artiste"></div>
      <h2 class="titre-musique">{{ actuel.titre }} - <span>{{ actuel.artiste }}</span></h2>
      <div class="boutons">
        <button class="precedente" @click="precedente"><i class="fa-solid fa-backward fa-2xl"></i></button>
        <button class="commencer" v-if="!joue" @click="play(actuel)"><i class="fa-solid fa-play fa-2xl"></i></button>
        <button class="pause" v-else @click="pause"><i class="fa-solid fa-pause fa-2xl"></i></button>
        <button class="suivante" @click="suivante"><i class="fa-solid fa-forward fa-2xl"></i></button>
      </div>
    </section>
    <section class="liste-de-lecture">
      <h3>Liste de Lecture</h3>
      <button v-for="musique in musiques" :key="musique.src" @click="play(musique)" :class="(musique.src == actuel.src) ? 'musique joue' : 'musique'">{{ musique.titre }} - {{ musique.artiste }}</button> 
    </section>
   </main>
    </div> 
   
  </div>
  
</template>


<script>
//import michaelJacksonPyt from './assets/musics/michael-jackson*pyt.mp3';
//import theWeekndPopular from './assets/musics/the-weeknd*popular.mp3';

/**
 * La classe app contient la propriété data pour crée les tableaux avec l'information nécessaire
 * à l'aide des méthodes la musique peut jouer, s'arreter, revenir à la musique d'avant, 
 * puis sauter à la musique d'après, created est là puisqu'il crée un nouveau composant
 * 
 */
export default {//fonction pour créer le tableau, faire changer la musique, mettre pause, definir la musique actuelle.
  name: 'app',
  data() { //créer un tableau pour les différents musiques avec les différentes informations pour chacune des musiques
    return {
      actuel: {},
      index: 0,
      joue: false,
      musiques: [
        {
          titre:'P.Y.T',
          artiste: 'Michael Jackson',
          src: '/src/assets/musics/michael-jackson*pyt.mp3',
          image:'/src/assets/photos/mj.jpeg'
        },
        {
          titre:'Popular',
          artiste: 'The Weeknd',
          src: '/src/assets/musics/the-weeknd*popular.mp3',
          image:'/src/assets/photos/weeknd.jpeg'
        },
        {
          titre:'Le temps',
          artiste: 'Tayc',
          src: '/src/assets/musics/tayc*le-temps.mp3',
          image:'/src/assets/photos/tayc.jpeg'
        },
        {
          titre:'Bring Me To Life',
          artiste: 'Evanescence',
          src: '/src/assets/musics/evanescence*bring-me-to-life.mp3',
          image:'/src/assets/photos/evanescence.jpeg'
        }
        
      ],
      lecteur: new Audio()
    }
  },
   methods:{
    play(musique){ //fonction pour commencer la musique
       // Arrêter la musique en cours s'il y en a une
      if (this.lecteur) {
        this.lecteur.pause();
        this.lecteur.currentTime = 0; // Réinitialiser la position de lecture
      }
      this.actuel = musique;
      document.getElementById('image-artiste').src = this.actuel.image; 
      this.lecteur = new Audio(this.actuel.src);
      //console.log(this.lecteur);
      this.lecteur.play();
      this.lecteur.addEventListener('ended',function (){ //quand la chanson s'arreter peut relancer de nouveau la chanson
        this.index++;
        if(this.index > this.musiques.length - 1){
        this.index = 0;
        }

        this.actuel = this.musiques[this.index];
        this.play(this.actuel)
      }.bind(this));
      this.joue =true;
    },
    pause () {// fonction pour arreter la musique
      this.lecteur.pause();
      this.joue = false;
    },
    precedente (){
      this.index--;
      if(this.index < 0){
        this.index = this.musiques.length - 1;
      }
      this.actuel = this.musiques[this.index];
      this.play(this.actuel);
    },
    suivante (){
      this.index++;
      if(this.index > this.musiques.length - 1){
        this.index = 0;
      }
      this.actuel = this.musiques[this.index];
      this.play(this.actuel);
    }
  },
  created () { // fonction pour créer un nouveau componant 
    this.actuel =this.musiques[this.index]; //definir la première musique dans mon tableau musiques
    //this.lecteur.src = this.actuel.src;//definition de la source de lecteur en utilisant la source de la musique joué
    //this.lecteur.play();
  }
}

</script>



<style scoped>
@import url("https://fonts.googleapis.com/css2?family=Material+Symbols+Outlined");
*{
  margin: 0;
  padding: 0;
  box-sizing: border-box;
}
.custom-cursor{
  background-color: rgb(40, 40, 40);
  background-image: url(./assets/logo.png);
  background-position: center;
  height: 100vh;
  width: 100vw;
  
}
body{
  font-family: sans-serif;
  background-color: rgba(133,141,143,0.8);
}
main{
  background-image: linear-gradient(to left bottom, rgba(31,30,30,100) rgba(1,0,15,100) );
  color: white;
  font-family: sans-serif;
}
header{
  display: flex;
  justify-content: center;
  align-items: center;
  padding: 15px;
  color: white;
  
}
header h1{
  text-align: center;
  font-weight: 900;
  text-shadow: 0.2rem 0.2rem black;

}
.lecteur,.liste-de-lecture{
margin-top: 1.5rem;
}
.liste-de-lecture{
  display: flex;
  flex-direction: column;
  text-align: center;
  text-shadow: 0.2rem 0.2rem black;
  margin-top: 1.5rem;
}
.liste-de-lecture h3{
  margin-bottom: 1.5rem;
}
.liste-de-lecture button{
  display: flex;
  align-items: center;
  justify-content: center;
  text-align: center;
  color: white;
  background: #C53551;
  width: 90%;
  height: 3rem;
  margin-left: auto;
  margin-right: auto;
  border: 0.15rem solid white;
}
.titre-musique{
  text-align: center;
  font-weight: 600;
  margin-top: 1.5rem;
  text-shadow: 0.2rem 0.2rem black;
}
.image-musique{
display: flex;
justify-content: center;
align-items: center;
margin: auto;
background-color: #C53551;
position: relative;
width:200px;
height: 200px;
overflow: hidden;
border-radius: 50%;
box-shadow: 0px 3px 12px rgba(0, 0, 0, 0.5) ;
background-position: center;
background-repeat: no-repeat;
background-size: cover;
animation: rotation 13s ease 3s infinite ; 
border: 0.2rem solid white;
}
.image-musique:hover{
  animation-play-state: paused;
}
.image-musique img {
  max-width: 100%;
  max-width: 100%;
  display: block;
  border-radius: 50%;
  border:0.1rem solid white;
}
.image-musique::after{
  content: '';
  display: block;
  padding-top: 100%;
}
.image-musique::before{
  content: '';
  display: flex;
  justify-content: center;
  align-items: center; 
  position: absolute;
  top:-3%;
  right:-3%;
  left:-3%;
  bottom:-3%;

  border:0.2rem solid white ;
  border-radius: 50%;
  box-shadow: 0px 1px 10px rgba(255, 255, 255, 0.8);

}
button{
  appearance: none;
  border: none;
  outline: none;
  background: none;
}

.boutons{
  display:flex;
  justify-content: space-evenly;
  align-items: center;
  text-align:center;
  margin-top: 1.5rem;
  
}
.commencer{
  background: #C53551;
  border-radius: 50%;
  width: 3.5rem;
  height: 3.5rem;
  border:0.1rem solid white;
}
.pause{
  background: #C53551;
  border-radius: 50%;
  width: 3.5rem;
  height: 3.5rem;
  border:0.1rem solid white;
  
}.precedente{
  background: #C53551;
  border-radius: 50%;
  width: 3.5rem;
  height: 3.5rem;
  border:0.1rem solid white;
}.suivante{
  background: #C53551;
  border-radius: 50%;
  width: 3.5rem;
  height: 3.5rem;
  border:0.1rem solid white;
}
/* animations */
@keyframes rotation {
  from{
    transform: rotate(1deg);
  }
  to{
    transform: rotate(365deg);
  }
}
</style>
