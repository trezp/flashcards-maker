<script>

export default {
  props: ["front", "back"]
}
</script>

<template>
  <div>
    <h1>I am a card</h1>
    <div>{{ front}} </div>
    <div>{{ back }} </div>
  </div>
</template>

<style scoped>

</style>
