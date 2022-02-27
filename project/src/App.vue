<script setup>
import { ref, computed, reactive } from "vue";

// <---- Main System ---->
const status_page = ref('select_game');

const sound = ()=>{
  const audio = new Audio('src/assets/song.mp3');  
  audio.volume = 0.05;
  audio.loop = true;
  audio.play(); 
}

//Name
const username = ref('')

// <----  ---->

// <---- Memory Challenge ---->
//Status Page
const backtoMemoryChallenge = ref(() => {
  document.body.style.backgroundColor = 'white';
  status_page.value = 'memory_challenge'
  answer_MemoryChallenge.value = undefined
  point.value = 0
});

//Disable button-Start
const disabledButton_startMemoryChallenge = computed(()=>{
  return level_MemoryChallenge.value === 0? true : false
})

//Point Calculate
let point_MemoryChallenge = ref(0);

//Progress Bar
const progress_bar_MemoryChallenge = ref(undefined);

//Random Number
const remember_number = ref(0);
const respond_MemoryChallenge = ref(0);
const range_MemoryChallenge = ref(100000);
const degit_MemoryChallenge  = ref(1000);

const generateNumber = (range , degit) =>{
  return (Math.floor(Math.random() * range) + degit)
};

const start_MemoryChallenge = () =>{
  const randNum = generateNumber(range_MemoryChallenge.value , degit_MemoryChallenge.value);
  status_page.value = 'process_memorychallenge'
  remember_number.value = randNum;
  respond_MemoryChallenge.value = randNum;
  setTimeout(() => { remember_number.value = 0; status_page.value = 'answer_memorychallenge' }, level_MemoryChallenge.value)
  progress_bar_MemoryChallenge.value = selectLevel();
  answer_MemoryChallenge.value = undefined
}

//Select Level
const level_MemoryChallenge = ref(0);
const selectLevel = () => {
  if (level_MemoryChallenge.value == 5000) {
    document.body.style.backgroundColor = '#E4FFCF';
    return 'play-animation-easy';
  } else if (level_MemoryChallenge.value == 2500) {
    document.body.style.backgroundColor = '#FCFFCF';
    return 'play-animation-normal';
  } else {
    document.body.style.backgroundColor = '#FFCFCF';
    return 'play-animation-hard';
  }
};

//Checking Answer Input
const answer_MemoryChallenge = ref(0)
const backup_ans = ref(0);

const check_MemoryChallenge = () => {
  backup_ans.value = answer_MemoryChallenge.value;
  if (answer_MemoryChallenge.value == respond_MemoryChallenge.value) {
    point_MemoryChallenge.value++;
    status_page.value = 'rest_memorychallenge';
    return true;
  } else {
    status_page.value = 'summary_memorychallenge';
    remember_number.value = 0;
    return false;
  }
}
// <----  ---->

// <---- Math Challenge ---->

//Math Calculate
const range_MathChallenge = ref(10)
const degit_MathChallenge = ref(0)
const operator = reactive(['+','*','-']);
const proposition = ref('');
const answer_MathChallenge = ref(0);
let array_choice = reactive([]);
let choice_MathChallenge = reactive([]);

//Start Math Game
let start_MathChallenge = () =>{
  const randNum1 = generateNumber(range_MathChallenge.value , degit_MathChallenge.value);
  const randNum2 = generateNumber(range_MathChallenge.value , degit_MathChallenge.value);
  const randOperator = operator[generateNumber(3,0)];
  proposition.value = `${randNum1}${randOperator}${randNum2}`
  answer_MathChallenge.value = eval(proposition.value);
  status_page.value = 'process_mathchallenge'
  createChoice(eval(proposition.value), generateChoice());
  choice_MathChallenge[0] = array_choice[0];
  choice_MathChallenge[1] = array_choice[1];
  setTimeout(()=>{
    if(user_answer_MathChallenge.value == true || user_answer_MathChallenge.value == false){
      user_answer_MathChallenge.value = undefined;
      progress_bar_MathChallenge.value = 'play-animation-math';
    }else{
      status_page.value = 'summary_mathchallenge';
    }
  },1750)
}

//Point
const point_MathChallenge = ref(0);

//Genarate Choice
let generateChoice = ()=>{
  if(degit_MathChallenge.value == 0){
    return (generateNumber(range_MathChallenge.value , degit_MathChallenge.value+10));
  }else if(degit_MathChallenge.value == 10){
    return (generateNumber(range_MathChallenge.value , degit_MathChallenge.value+1000));
  }else
    return (generateNumber(range_MathChallenge.value , degit_MathChallenge.value+100000));
}

//Create Choice
const createChoice = (answers , choices)=>{
  array_choice = [undefined,undefined];
  array_choice[generateNumber(2,0)] = answers;
  if(array_choice[0] == undefined){
    array_choice[0] = choices
  }else
    array_choice[1] = choices
}

//Checking Answer
let user_answer_MathChallenge = ref();
const isCorrect = ()=>{
  if(answer_MathChallenge.value == choice_MathChallenge[0]){
    return true;
  }else;
    return false;
}
let check_MathChallenge = ()=>{
  if(user_answer_MathChallenge.value == isCorrect()){
    point_MathChallenge.value++;
    progress_bar_MathChallenge.value = '';
    start_MathChallenge();
    return true;
  }else{
    status_page.value = 'summary_mathchallenge';
    return false;
  }
}

//Reset Math Game
const backtoMathChallenge = ()=>{
  status_page.value = 'math_challenge'
  point_MathChallenge.value = 0
}

//Progress Bar : Math Game
let progress_bar_MathChallenge = ref('play-animation-math');
// <----  ---->

//CSS
const bg_main = computed(()=>{
  return status_page.value == `select_game`? 'wrapper' : '';
})


</script>

<template>
<div :id="bg_main">
   <div id="div-2" v-if="status_page == `select_game`" >
     <h2 class="welcome">Welcome to the land of games.</h2>
     <h4 class="welcome">Please Select Games</h4> <br>
        <!-- <button class="btn btn-warning btn-lg active" @click="status = `memorygame`">Memory Challenge</button> -->
        <!-- <button class="btn btn-warning btn-lg active" @click="status = `mathgame`">Math Challenge</button> -->
        <div class="button">
        <input v-model="username" style="font-size: 27px; text-align: center;" placeholder="username"><br>
        <br>
        <button class="btn btn-warning btn-lg active" @click="status_page = `memory_challenge`; sound();">Memory Challenge</button>&nbsp;&nbsp;
        <button class="btn btn-warning btn-lg active" @click="status_page = `math_challenge`; sound();">Math Challenge</button>&nbsp;&nbsp;
        <!-- <button class="btn btn-warning btn-lg active" >Meditaion Challenge</button>&nbsp;&nbsp;
        <button class="btn btn-warning btn-lg active" >Reaction Challenge</button>&nbsp;&nbsp; -->
        </div>
    </div>
    <!-- Start Memory Game Page -->
    <div id="first-game">
          <div class="title" v-if="status_page == `memory_challenge`" style="margin-top: 3.5em;">
      <h1>Memory Challenge</h1>
      <div class="intro">
        <h5>Can you remember these 4-6 numbers ? Let's try !!!</h5>
      </div><br>
      <div class="button">
        <br>
        <div class="level">
        <button type="button" class="btn btn-outline-success" @click="level_MemoryChallenge = 5000" style="margin-right: 10px;">Easy</button>
        <button type="button" class="btn btn-outline-warning" @click="level_MemoryChallenge = 2500" style="margin-right: 10px;">Normal</button>
        <button type="button" class="btn btn-outline-danger"  @click="level_MemoryChallenge = 1000" style="margin-right: 10px;">Hard</button>
      </div><br>    
        <button class="btn btn-warning btn-lg active" @click="start_MemoryChallenge" :disabled="disabledButton_startMemoryChallenge">Start</button>&nbsp;&nbsp;
        <button class="btn btn-warning btn-lg active" @click="status_page = 'select_game'">Back to home</button>
      </div>
    </div>
    <!-- ------ -->

    <!-- Memory Challenge : Process -->
    <div style="margin-top: 3em; margin-bottom: 3.5em;" v-if="status_page == `process_memorychallenge`">
      <h1>{{ remember_number }}</h1>
      <br>
      <div class="progress-bar" :id="progress_bar_MemoryChallenge"></div>
    </div>

    <div style="margin-top: 3em; margin-bottom: 3.5em;" v-if="status_page == `answer_memorychallenge`">
      <h2>What was the number?</h2>
      <h4>Press enter to submit</h4>
      <input v-model="answer_MemoryChallenge" @keyup.enter="check_MemoryChallenge" style="font-size: 50px; text-align: center;"/>
      <br>
      <br>
      <button class="btn btn-warning btn-lg active" @click="check_MemoryChallenge" style="font-size: 160%;">submit</button>
    </div>
    <!-- ------ -->

    <!-- Memory Challenge : Rest -->
    <div style="margin-top: 3.5em; margin-bottom: 3.5em;" v-if="status_page == `rest_memorychallenge`">
      <h2> --- {{username}} --- </h2>
      <h2>Answer is : {{ respond_MemoryChallenge }}</h2>
      <h2>Your answer is : {{ backup_ans }}</h2>
      <h3>Point : {{ point_MemoryChallenge }}</h3>
      <br>
      <button class="btn btn-warning btn-lg active" @click="start_MemoryChallenge">Next</button>
    </div>
    <!-- ------ -->

    <!-- Memory Challenge : Summary -->
    <div style="margin-top: 3.5em; margin-bottom: 3.5em;" v-if="status_page == `summary_memorychallenge`">
      <h2> --- {{username}} --- </h2>
      <h2>Nice Try !</h2>
      <h2>Your points : {{ point_MemoryChallenge }}</h2>
      <br>
      <button class="btn btn-warning btn-lg active" @click="backtoMemoryChallenge">Back to home</button>
    </div>
    <!-- ------ -->

    </div>

    <!-- Math Challenge : Start Page -->
    <div id= "second-game">
    <div class="title" v-if="status_page == `math_challenge`" style="margin-top: 1.75em;">
        <h1>Math Challenge</h1>
        <div class="intro">
          <h5>Will you be able to Calculate the numbers in the given time?</h5>
        </div><br>
        <div class="level">
        <button type="button" class="btn btn-outline-success" @click="range_MathChallenge = 10; degit_MathChallenge = 0" style="margin-right: 10px;">1 Degit</button>
        <button type="button" class="btn btn-outline-warning" @click="range_MathChallenge = 90; degit_MathChallenge = 10" style="margin-right: 10px;">2 Degit</button>
        <button type="button" class="btn btn-outline-danger"  @click="range_MathChallenge = 900; degit_MathChallenge = 100" style="margin-right: 10px;">3 Degit</button>
      </div><br>
      <div class="button">
          <button class="btn btn-warning btn-lg active" @click="start_MathChallenge">Start</button>&nbsp;&nbsp;
          <button class="btn btn-warning btn-lg active" @click="status_page = 'select_game'">Back to home</button>
        </div>
    </div>
    <!-- ------ -->

    <!-- Math Challenge : Process -->
    <div style="margin-top: 2em;" v-if="status_page == `process_mathchallenge`">
      <h1>{{ proposition }}</h1>
      <div class="progress-bar" :id="progress_bar_MathChallenge"></div>
      <br>
      <button class="btn btn-secondary btn-lg active" @click="user_answer_MathChallenge = true; check_MathChallenge();">{{ choice_MathChallenge[0] }}</button>&nbsp;&nbsp;
      <button class="btn btn-secondary btn-lg active" @click="user_answer_MathChallenge = false; check_MathChallenge();">{{ choice_MathChallenge[1] }}</button>
    </div>
    <!-- ------ -->

    <!-- Math Challenge : Summary -->
    <div style="margin-top: 2em;" v-if="status_page == `summary_mathchallenge`">
      <h2>Nice Try !</h2>
      <h2>Your points : {{ point_MathChallenge }}</h2>
      <br>
      <button class="btn btn-warning btn-lg active" @click="backtoMathChallenge">Back to home</button>
    </div>
    </div>
    <!-- ------ -->

  </div>


</template>

<style> 
#wrapper {
  margin-top: -6em;
  padding-top: 7em;
  background-color: #2d4057 ;
  position: relative;
}
#div-2 {
  background-color: #4097aa;
  padding: 200px;
  margin-top: 87px;
  margin-left: 50px;
  margin-right: 50px;
 }
.welcome{
  text-align: center;
  color: aliceblue;
}
.button{
  text-align: center;
}
.container {
  justify-content: center;
  align-items: center;
  cursor: pointer;
  text-align: center;
  height: 15px;
  position: relative;
}
.title h1 {
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
.intro{
  text-align: center;
}
.level{
  text-align: center;
}
#first-game{
  /* background-color: bisque; */
  text-align: center;
  padding: 90px;
  margin-top: 80px;
  margin-left: 50px;
  margin-right: 50px;
}

#second-game{
  text-align: center;
  padding: 30px;
  /* margin-top: 80px; */
  margin-left: 50px;
  margin-right: 50px;
}
#play-animation-easy {
  animation: progress-animation 5s forwards;
  size: 1.5ch;
  height: 10px;
  background-color: green;
  
}

#play-animation-normal {
  animation: progress-animation 2.5s forwards;
  size: 1.5ch;
  height: 10px;
  background-color: yellow;
}

#play-animation-hard {
  animation: progress-animation 1s forwards;
  size: 1.5ch;
  height: 10px;
  background-color: red;
}

#play-animation-math {
  animation: progress-animation 1.75s forwards;
  size: 1.5ch;
  height: 10px;
  background-color:#F77E4E ;
}
#wrapper {
  margin-top: -6em;
  padding-top: 7em;
  background-color: #2d4057 ;
  position: relative;
}
#div-2 {
  background-color: #4097aa;
  padding: 200px;
  margin-top: 80px;
  margin-left: 50px;
  margin-right: 50px;
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