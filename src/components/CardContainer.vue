<script>
import { initializeApp } from "firebase/app";
import { getFirestore } from "firebase/firestore";
import { collection, getDocs } from "firebase/firestore"; 

import { ref } from 'vue'

import Card from './Card.vue';

export default {
  components: {
    Card
  },
  methods: {
  
  },
  setup(){
    initializeApp({
      apiKey: process.env.FIREBASE_API_KEY,
      authDomain: "flashcards-vue.firebaseapp.com",
      projectId: "flashcards-vue",
      storageBucket: "flashcards-vue.appspot.com",
      messagingSenderId: process.env.FIREBASE_SENDER_ID,
      appId: process.env.FIREBASE_APP_ID,
      measurementId: process.env.FIREBASE_MEASUREMENT_ID
    });

    const db = getFirestore(); 
    let cards = ref([])
    
    const getCards = async() => {
      const querySnapshot = await getDocs(collection(db, "cards"));
      querySnapshot.forEach((doc) => cards.value.push(doc.data()));
    }

    return {
      cards,
      getCards
    }
  },
  mounted () {
    this.getCards() 
  }
}
</script>

<template>
  <h1>Flashcards</h1>
  <ul>
    <li v-for="card in cards" :key="card.front">
      <Card 
        :front="card.front"
        :back="card.back">
      </Card>
    </li>
  </ul>
</template>

<style scoped>

</style>
