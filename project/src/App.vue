<script setup>
import { ref, computed } from "vue";
//Random Number
const status = ref('none');
const showNumber = ref('');
const respond = ref('');
const generateNumber = () => {
  const randNumbers = Math.floor(Math.random() * 10000);
  status.value = 'ingame'
  showNumber.value = randNumbers;
  respond.value = randNumbers;
  setTimeout(() => { showNumber.value = '';status.value="ans" }, 2500)
};

//Answer Input
const ans = ref('')
function check() {
  if (ans.value == respond.value) {
    generateNumber();
    ans.value = ''
  } else {
    status.value = 'none';
    showNumber.value = '';
  }
}
const load = 'play-animation'

</script>

<template>
  <div class="container">
    <div class="title" v-if="status == `none`">
      Number Memory
      <div class="intro">The average person can remember 7 numbers at once. Can you do it more?</div>
      <div class="button">
        <button class="btn btn-warning btn-lg active" @click="generateNumber">Start</button>
      </div>
    </div>
    <h1>{{ showNumber }}</h1>
    <br>
    <div class="progress-bar" v-if="status == `ingame`" id="play-animation"></div>
    <p v-show="status == `ans`">What was the number?</p>
    <p v-show="status == `ans`">Press enter to submit</p>
    <div>
    <input class="form-control form-control-lg" type="text" 
    v-show="status == `ans`" v-model="ans" @keyup.enter="check">
    <br>
    <br>
    <button class="btn btn-warning btn-lg active" v-if="status == `ans`" @click="check">submit</button>
    </div>
  </div>
    
</template>

<style>
.container {
  justify-content: center;
  align-items: center;
  cursor: pointer;
  text-align: center;
  height: 15px;
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
  height: 75%;
  border-radius: 3px;
  background-color: brown;
}

#play-animation {
  animation: progress-animation 2.5s forwards;
  size: 1ch;
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
