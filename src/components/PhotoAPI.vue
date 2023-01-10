<script>
export default {
  data() {
    return {
      deckData: null,
      photoData: null,
      deckCardsData: [],
      random:Math.ceil(Math.random()*1000000),
      photoList: []
    };
  },
  methods: {
    fetchPhoto() {
      let cautare= document.getElementById("search").value;
      const Url =
        `https://api.unsplash.com/search/photos?query=${cautare}`;
        
        console.log(cautare);
      fetch(Url, {method:"GET", headers:{"Authorization":"Client-ID V4NrmuKVjRENpAk2hfE5Xf9XoXf3doTxS3Tn0-B7kTo"} })
        .then((response) => response.json())
        .then((data) => {
          console.log(data);
      this.photoData=data.results[0].urls.raw;
      console.log(this.photoData);
          
        });
    },


    fetchList(){
     let lungime;
     const Url=`https://api.unsplash.com/photos`;
     fetch(Url, {method:"GET", headers:{"Authorization":"Client-ID V4NrmuKVjRENpAk2hfE5Xf9XoXf3doTxS3Tn0-B7kTo"} })
        .then((response) => response.json())
        .then((data) => {
          lungime=data.lenght;
          for(let i=0; i<lungime;i++)
          {
            this.photoList[i]=data[i].urls.raw;
          }
          console.log(data);
      console.log(this.photoData);
    });
  },

  fetchRandomPhoto(photoId){
     const Url=`https://api.unsplash.com/photos/random`;
       
      fetch(Url, {method:"GET", headers:{"Authorization":"Client-ID V4NrmuKVjRENpAk2hfE5Xf9XoXf3doTxS3Tn0-B7kTo"} })
        .then((response) => response.json())
        .then((data) => {
          console.log(data);
      this.photoData=data.urls.raw;
      console.log(this.photoData);
          
        });
  },

    fetchCard(deckId) {
      const Url = `https://deckofcardsapi.com/api/deck/${deckId}/draw/?count=1`;
      fetch(Url)
        .then((response) => response.json())
        .then((data) => {
          this.cardData = data;
          this.deckCardsData = [];
        });
    },
    fetchAllCards(deckId, remaining) {
      const Url = `https://deckofcardsapi.com/api/deck/${deckId}/draw/?count=
        ${remaining}`;
      fetch(Url)
        .then((response) => response.json())
        .then((data) => {
          this.deckCardsData = data.cards;
        });
    },
  },
};
</script>

<template>
 
  <v-btn @click="fetchPhoto"
    >Get new photo <v-icon icon="mdi-plus"></v-icon
  ></v-btn>

  <v-btn @click="fetchList"
    >Get list <v-icon icon="mdi-plus"></v-icon
  ></v-btn>

  <v-btn @click="fetchRandomPhoto"
    >Get a random photo <v-icon icon="mdi-plus"></v-icon
  ></v-btn>

   

  <div v-if="deckData">
    <v-btn variant="outlined" @click="fetchCard(deckData.deck_id)"
      >Get my Card<v-icon icon="mdi-atom"></v-icon
    ></v-btn>
    <div v-if="cardData">
      <v-img
        class="bg-white"
        width="100"
        :aspect-ratio="1"
        :src="cardData.cards[0].image"
        v-if="this.photoData"
      />
    </div>
    <v-btn
      variant="tonal"
      @click="fetchAllCards(deckData.deck_id, deckData.remaining)"
    >
      Get all Cards <v-icon icon="mdi-auto-fix"></v-icon>
    </v-btn>

    <v-row v-if="deckCardsData.length > 0">
      <v-col
        v-for="(card, index) in deckCardsData"
        :key="index"
        class="d-flex child-flex"
        cols="2"
      >
        <v-img
          class="bg-grey-lighten-2"
          cover
          :aspect-ratio="0"
          :src="card.image"
        />
      </v-col>
    </v-row>
  </div>
  <v-img
          class="bg-grey-lighten-2"
          cover
          :aspect-ratio="0"
          :src="this.photoData"
          v-if="this.photoData"
        />
        

        <v-img v-for="image in photoList"
          class="bg-grey-lighten-2"
          cover
          :aspect-ratio="0"
          :src="image.urls.raw"
         
        />
      
        <v-text-field label="search" id = "search"></v-text-field>
</template>

<style scoped>
img {
  float: left;
  margin-right: -160px;
}
</style>
