<template>
  <li class="card" @click="flipCard">
    <div v-show="!flipped" class="card-front">{{ card.front}} </div>
    <div v-show="flipped" class="card-back">{{ card.back }} </div>
    <button @click="$emit('deleteCard', card.id)">Delete</button>
  </li>
</template>

<script>

export default {
  props: ["card"],
  emits: ["deleteCard"],
  methods: {
    flipCard(event) {
      event.stopPropagation();
      this.flipped = !this.flipped;
    }
  },
  data(){
    return {
      flipped: false
    }
  }
}
</script>



<style scoped lang="scss">
  .card {
    display: flex;
    justify-content: center;
    align-items: center;
    font-size: 1.5em;
    border: 1px solid black;
    height: 250px;
    width: 200px; 
    border-radius: 3px;
    margin-left: 30px;
  }
</style>
