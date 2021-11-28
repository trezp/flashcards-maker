<script>
import { initializeApp } from "firebase/app";
import { getFirestore } from "firebase/firestore";
import firebaseConfig from "../firebase.js";
import { collection, doc, query, onSnapshot, addDoc, deleteDoc, Timestamp, orderBy } from "firebase/firestore"; 

import Card from './Card.vue';

export default {
  components: {
    Card
  },
  methods: {
    async createCard(){
      this.cards = [];
      await addDoc(collection(this.db(), "cards"), {
        front: this.front,
        back: this.back,
        timestamp: Timestamp.fromDate(new Date())
      });
      this.front = '';
      this.back = '';
    },
    async deleteCard(id){
      this.cards = [];
      await deleteDoc(doc(this.db(), "cards", id));
    }
  },
  mounted() {
    initializeApp(firebaseConfig);

    const cardsRef = collection(this.db(), "cards");
    const q = query(cardsRef, orderBy("timestamp"));

    this.unsubscribe = onSnapshot(q, (querySnapshot) => {
      querySnapshot.forEach(doc => {
        this.cards.push({...doc.data(), id: doc.id, timestamp: Timestamp.fromDate(new Date())})
      });
    })
  },
  unmounted(){
    this.unsubscribe();
  },
  data(){
    return {
      db: getFirestore,
      cards: [],
      unsubscribe: null,
      front: '',
      back: ''
    }
  }
}
</script>

<template>
  <h1>Flashcards</h1>
  <h3>Add New Card</h3>
  <input type="text" placeholder="Front" v-model="front">
  <input type="text" placeholder="Back" v-model="back">
  <button @click="createCard(front, back)">Add Card</button>
  <ul class="cardList">
    <Card 
      @deleteCard="deleteCard"
      v-for="card in cards" :key="card.front"
      :card="card">
    </Card>
  </ul>
</template>

<style scoped lang="scss">

.cardList {
  display: flex;
  flex-wrap: wrap;
  li {
    list-style-type: none;
  }
}

</style>
