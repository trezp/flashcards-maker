<script>
import { initializeApp } from "firebase/app";
import { getFirestore } from "firebase/firestore";
import { collection, doc, addDoc, getDocs, deleteDoc } from "firebase/firestore"; 

import { ref} from 'vue'

import Card from './Card.vue';

export default {
  components: {
    Card
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
    const cards = ref([]);

    const getCards = async() => {
      const querySnapshot = await getDocs(collection(db, "cards"));
      querySnapshot.forEach((doc) => cards.value.push({...doc.data(), "id": doc.id}));
      
    }

    const addCard = async(front, back) => {
      console.log(front, back)
      await addDoc(collection(db, "cards"), {
        front: front,
        back: back
      });

      cards.value = [];
      getCards();
    }

    const deleteCard = async(id) => {
      await deleteDoc(doc(db, "cards", id));
      cards.value = [];
      getCards();
    }

    return {
      cards,
      addCard,
      getCards,
      deleteCard
    }
  },
  methods: {
    createCard(){
      this.addCard(this.frontCard, this.backCard);
      this.front = '';
      this.back = '';
    }
  },
  mounted() {
    this.getCards();
  },
  data(){
    return {
      front: '',
      back: ''
    }
  }
}
</script>

<template>
  <h1>Flashcards</h1>
  <h3>Add New Card</h3>
  <input type="text" placeholder="Front" v-model="frontCard">
  <input type="text" placeholder="Back" v-model="backCard">
  <button @click="createCard(frontCard, backCard)">Add Card</button>
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
