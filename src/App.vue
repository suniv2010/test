<template>
  <div id="app">
   <h1 class="title">
    Shuffle and Sort
  </h1>
  <br/>
  <div class="deck">
  <transition-group :name="shuffleSpeed" tag="div" v-if="$mq !== 'md'">
    <div v-for="(card) in cards" :key="card.id"
         class="card"
         v-bind:style="{ background: card.color }">   
      <span class="card__number">{{ card.rank }}</span>      
    </div>   
  </transition-group>
  <transition-group :name="shuffleSpeed" tag="div" v-if="$mq === 'md'">
     <div media="(max-width: 600px)" v-for="(card) in cards" :key="card.id"
         class="card"
         v-bind:style="{ background: '#EFEFEF',borderLeft : '10px solid', borderColor: card.color }">
   
      <span class="card__number">{{ card.rank }}</span>
      
    </div>  
  </transition-group>
  
  </div>
  <div class="main-buttons">
    <button  @click="displayInitialDeck" class="button is-primary" style="background-color:#72C3DC">
      Sort <i class="fas fa-undo"></i>
    </button>
    <button @click="shuffleDeck" class="button is-primary" style="background-color:#72C3DC">
      Shuffle <i class="fas fa-random"></i>
    </button>
  </div>
</div>
</template>

<style lang="css">
@import url("https://fonts.googleapis.com/css?family=Roboto+Slab:300,400,700");

html, body, #app {
  height: 100%;
  background: ghostwhite;
   display: flex;
  align-items: center;
  justify-content: center;
}

.title {
  font-family: Roboto Slab, sans-serif;
  text-align: center;
  position: absolute;
  margin-bottom: 0 !important;
  font-weight: 300;
  font-size: 3rem;
      margin-top: -178px;
}

.vue-logo {
  height: 55px;
  position: relative;
  top: 10px;
}

.speed-buttons {
  text-align: center;
  padding-top: 30px;
}
.speed-buttons .button {
  height: 2.50em;
}


.main-buttons {
    /* display: block; */
    width: 100px;
    /* margin: 0 auto; */
    text-align: center;
    /* padding: 30px; */
    /* position: relative; */
    margin-top: -209px !important;
}

.count-section {
  position: absolute;
  top: 10px;
  right: 10px;
}

.fas {
  padding-left: 5px;
}

.deck {
  width:840px;
  margin-left: 30px;
  padding-top: 30px;
}

.card {
  width: 275px;
  height: 100px;
  float: left;
  margin-right: 5px;
  margin-bottom: 5px;
  border-radius: 2px;
}

.card__number {
 width: 100%;
    position: absolute;
    top: 20%;
    text-align: center;
    color: #fff;
    font-size: 3em;
}

.shuffleSlow-move {
  transition: transform 2s;
}

.shuffleMedium-move {
  transition: transform 1s;
}

.shuffleFast-move {
  transition: transform 0.5s;
}

.fade-enter-active, .fade-leave-active {
  transition: opacity .5s;
}

.fade-enter, .fade-leave-to {
  opacity: 0;
}
.button.is-primary {
    width: 97px !important;
    margin: 2px;
    background-color: '#72C3DC' !important;
}
@media (max-width: 800px) {
  .deck {
    flex: 50%;
    max-width: 50%;
    max-height:30%;
  }
  .card {
  width: 275px;
  height: 80px;
  float: left;
  margin-right: 5px;
  margin-bottom: 5px;
  border-radius: 2px;
}
.main-buttons {
    display: inherit;
    margin: 0 auto;
    text-align: center;
    padding: -53px;
    position: absolute;
    margin: -73px !important;
}
.title {
    font-family: Roboto Slab, sans-serif;
    text-align: center;
    position: absolute;
    margin-bottom: 0 !important;
    font-weight: 300;
    font-size: 3rem;
    margin-top: -137px;
    margin-left: -49px;
}
.card__number {
 width: 100%;
    position: absolute;
    top: 20%;
    text-align: center;
    color:#2F454E;
    font-size: 3em;
}
}

</style>
<script>
import Vue from 'vue';
import VueMq from 'vue-mq'
Vue.use(VueMq, {
  breakpoints: {
    sm: 450,
    md: 800,
    lg: 1250,
  }
})

export default {
    name: 'app',
  data() {
    return {
    ranks: ['1', '2', '3', '4', '5', '6', '7', '8', '9'],
    suits: [''],
    cards: [],
    suitColor: {      
      '1': '#2B8EAD ',     
      '3': '#BFBFBF',     
      '2': '#2F454E ',
      '5': '#2B8EAD ',     
      '4': '#BFBFBF',     
      '6': '#2F454E ',
      '9': '#2B8EAD ',     
      '8': '#BFBFBF',     
      '7': '#2F454E '      
    },
    shuffleSpeed: 'shuffleMedium',
    shuffleTypes: ['Slow', 'Medium', 'Fast'],
    isDeckShuffled: false,
    shuffleCount: 0,
    }
  },
  mounted() {
    this.displayInitialDeck();
  },  
  methods: {
    displayInitialDeck() {
      let id = 1;
      this.cards = [];

      for( let s = 0; s < this.suits.length; s++ ) {
        for( let r = 0; r < this.ranks.length; r++ ) {
          let card = {
            id: id,
            rank: this.ranks[r],
            suit: this.suits[s],
            color:this.suitColor[id]
          }
          this.cards.push(card);
          id++;
        }
      }
      this.isDeckShuffled = false;
      this.shuffleCount = 0;
    },
    shuffleDeck() {        
      for(let i = this.cards.length - 1; i > 0; i--) {
        let randomIndex = Math.floor(Math.random() * i);        
        let temp = this.cards[i];
        Vue.set(this.cards, i, this.cards[randomIndex]);
        Vue.set(this.cards, randomIndex, temp);
      }
      this.isDeckShuffled = true;
      this.shuffleCount = this.shuffleCount + 1;
    }
  }
}; 
</script>