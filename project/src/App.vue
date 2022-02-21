<script setup>
import { ref, computed } from "vue";

//Random Number
const randNumbers = ref("");
const generateNumber = () => {
  const num = Math.floor(Math.random() * 100);
  randNumbers.value = num;
};

//Answer Input
const ans = ref(' ')
function check() {
  if (ans.value == randNumbers.value) {
    generateNumber();
    ans.value = ''
  }
}

</script>

<template>
  <div class="container">
    <div class="title">
      Number Memory
      <div class="intro">The average person can remember 7 numbers at once. Can you do it more?</div>
      <div class="button">
        <button class="btn btn-warning btn-lg active" @click="generateNumber">Start</button>
        <h1>{{ randNumbers }}</h1>
        <!-- ** need correction ** -->
        <input v-if="randNumbers != ``" v-model="ans" />
        <button v-if="randNumbers != ``" @click="check">submit</button>
      </div>
    </div>
  </div>
</template>

<style>
.container {
  background-color: cornflowerblue;
  justify-content: center;
  align-items: center;
  cursor: pointer;
}
.title {
  font-size: 50px;
  text-align: center;
}
.intro {
  font-size: 25px;
  text-align: center;
}
.button {
  text-align: center;
}
</style>