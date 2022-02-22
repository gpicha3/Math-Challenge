<script setup>
import { ref, computed } from "vue";
//Status Page
const status = ref('none');
const reset = ref(()=>{
  status.value = 'none'
  ans.value = ''
  point.value = 0 
});
//Point Calculate
let point = ref(0);

//Random Number
const showNumber = ref('');
const respond = ref('');
const generateNumber = () => {
  const randNumbers = Math.floor(Math.random() * 100000 + 1000);
  status.value = 'ingame'
  showNumber.value = randNumbers;
  respond.value = randNumbers;
  setTimeout(() => { showNumber.value = ''; status.value = 'ans' }, 2500)
  ans.value = ''
};

//Checking Answer Input
const ans = ref('')
const ans_backup = ref('');

function check() {
  ans_backup.value = ans.value;
  if (ans.value == respond.value) {
    point.value++;
    status.value = 'break';
    console.log('Point is :' + point.value);
    return true;
  }else{
    status.value = 'back';
    showNumber.value = '';
    console.log('Point is :' + point.value);
    console.log(status.value);
    return false;
  }
}
//Progress Bar
const progress_bar = 'play-animation'

</script>

<template>
  <div class="container">

    <!-- Menu Page -->
    <div class="title" v-if="status == `none`">
      Number Memory
      <div class="intro">
        <h1>The average person can remember 7 numbers at once. Can you do it more?</h1>
      </div>
      <div class="button">
        <button class="btn btn-warning btn-lg active" @click="generateNumber">Start</button>
      </div>
    </div>
    <!-- ------ -->

    <!-- inGame Page -->
    <h1 v-show="status == `ingame`">{{ showNumber }}</h1>
    <br />
    <div class="progress-bar" v-if="status == `ingame`" id="play-animation"></div>
    <h2 v-show="status == `ans`">What was the number?</h2>
    <h4 v-show="status == `ans`">Press enter to submit</h4>
    <input v-show="status == `ans`" v-model="ans" @keyup.enter="check" style="font-size: 50px; text-align: center;"/>
    <br>
    <br>
    <button class="btn btn-warning btn-lg active" v-if="status == `ans`" @click="check" style="font-size: 160%;">submit</button>
    <!-- ------ -->
  
    <!-- Break Page -->
    <h2 v-show="status ==`break` ">Answer is : {{respond}}</h2>
    <h2 v-show="status ==`break` ">Your answer is : {{ans_backup}}</h2>
    <h3 v-show="status ==`break` ">Point : {{point}}</h3>
    <br>
    <button v-show="status ==`break`" class="btn btn-warning btn-lg active" @click="generateNumber">Next</button>
    <!-- ------ --> 

    <!-- Summary Page -->
    <h2 v-show="status ==`back`">Nice Try ! </h2>
    <h2 v-show="status ==`back`">Your points : {{point}}</h2>
    <br>
    <button v-show="status ==`back`" class="btn btn-warning btn-lg active" @click="reset" >Back to home</button>
    <!-- ------ --> 
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
.intro h1 {
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