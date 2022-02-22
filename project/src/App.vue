<script setup>
import { ref, computed } from "vue";
//Random Number
const randNumbers = ref('');
const randNumbers_backup = ref('');
const generateNumber = () => {
  const num = Math.floor(Math.random() * 10000);
  randNumbers.value = num;
  randNumbers_backup.value = num;
  setTimeout(() => { randNumbers.value = ' ' }, 4500)
};

//Answer Input
const ans = ref('')

function check() {
  if (ans.value == randNumbers_backup.value) {
    generateNumber();
    ans.value = ''
  } else {
    randNumbers.value = '';
    ans.value = ''
  }
}

const load = 'play-animation'

</script>

<template>
  <div class="container">
    <div class="title" v-if="randNumbers == ``">
      Number Memory
      <div class="intro">The average person can remember 7 numbers at once. Can you do it more?</div>
      <div class="button">
        <button class="btn btn-warning btn-lg active" @click="generateNumber">Start</button>
      </div>
    </div>
    <h1>{{ randNumbers }}</h1>
    <input v-if="randNumbers != ``" v-model="ans" @keyup.enter="check" />
    <button v-if="randNumbers != ``" @click="check" >submit</button>
    <br>
    <br>
    <div class="progress-bar progress-bar-striped progress-bar-animated" v-if="randNumbers != ``" id="play-animation"></div>

  
  </div>
</template>

<style>
.container {
  justify-content: center;
  align-items: center;
  cursor: pointer;
  text-align: center;
  height: 25px;
  position: relative;
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
.container .progress-bar {
  position: absolute;
  height: 100%;
  border-radius: 7px;
  background-color: greenyellow;
}

#play-animation {
  animation: progress-animation 4.5s forwards;
}

@keyframes progress-animation {
  0% {
    width: 0%;
  }
  100% {
    width: 100%;
  }
}
</style>