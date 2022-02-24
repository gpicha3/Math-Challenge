<script setup>
import {
    ref,
    computed
} from "vue";

//Status Page
const status = ref('none');
const reset = ref(() => {
    status.value = 'none'
    ans.value = ''
    point.value = 0
    level.value = 0
});

//Disable button-Start
const disabledButton = computed(() => {
    return level.value === 0 ? true : false
})

//Point Calculate
let point = ref(0);

//Progress Bar
const progress_bar = ref('');

//Random Number
const showNumber = ref('');
const respond = ref('');
const generateNumber = () => {
    const randNumbers = Math.floor(Math.random() * 100000) + 1000;
    status.value = 'ingame'
    showNumber.value = randNumbers;
    respond.value = randNumbers;
    setTimeout(() => {
        showNumber.value = '';
        status.value = 'ans'
    }, level.value)
    progress_bar.value = selectLevel();
    ans.value = ''
};

//Select Level
const level = ref(0);
const selectLevel = () => {
    if (level.value == 5000) {
        return 'play-animation-easy';
    } else if (level.value == 2000) {
        return 'play-animation-normal';
    } else {
        return 'play-animation-hard';
    }
};

//Checking Answer Input
const ans = ref('')
const ans_backup = ref('');

const check = () => {
    ans_backup.value = ans.value;
    if (ans.value == respond.value) {
        point.value++;
        status.value = 'break';
        return true;
    } else {
        status.value = 'back';
        showNumber.value = '';
        return false;
    }
}

//Name
const name = ref('')

</script>

<template>
<div class="container ">
  
    <!-- Menu Page -->
    <div class="title" v-if="status == `none`" style="margin-top: 3em;">
        Number Memory
        <div class="intro">
            <h1>The average person can remember 7 numbers at once. Can you do it more?</h1>
        </div>
        <input v-if="status == `none`" v-model="name" style="font-size: 20px; text-align: center;" placeholder="UserName :">
        <div>
            <button type="button" class="btn btn-outline-success" @click="level = 5000" style="margin-right: 10px;">Easy</button>
            <button type="button" class="btn btn-outline-warning" @click="level = 2000" style="margin-right: 10px;">Normal</button>
            <button type="button" class="btn btn-outline-danger" @click="level = 1000" style="margin-right: 10px;">Hard</button>
        </div>
        <div class="button">
            <button class="btn btn-warning btn-lg active" @click="generateNumber" :disabled="disabledButton">Start</button>
        </div>
    </div>
    <!-- ------ -->

    <!-- inGame Page -->
    <div style="margin-top: 10em;">
        <h1 v-show="status == `ingame`">{{ showNumber }}</h1>
        <br />
        <div class="progress-bar" v-if="status == `ingame`" :id="progress_bar"></div>
        <h2 v-show="status == `ans`">What was the number?</h2>
        <h4 v-show="status == `ans`">Press enter to submit</h4>
        <input v-if="status == `ans`" v-model="ans" @keyup.enter="check" style="font-size: 40px; text-align: center;" />
        <br />
        <br />
        <button class="btn btn-warning btn-lg active" v-if="status == `ans`" @click="check" style="font-size: 160%;">submit</button>
    </div>
    <!-- ------ -->

    <!-- Break Page -->
    <h4 v-show="status == `break`">--- Username : {{name}} --- </h4>
    <h2 v-show="status == `break`">Answer is : {{ respond }}</h2>
    <h2 v-show="status == `break`">Your answer is : {{ ans_backup }}</h2>
    <h3 v-show="status == `break`">Point : {{ point }}</h3>
    <br />
    <button v-show="status == `break`" class="btn btn-warning btn-lg active" @click="generateNumber">Next</button>
    <!-- <div><br>
    <button v-show="status == `break`" class="btn btn-warning btn-lg active" @click="reset">Back to home</button>
    </div> -->
    <!-- ------ -->

    <!-- Summary Page -->
    <h2 v-show="status == `back`">--- {{name}} --- </h2>
    <h2 v-show="status == `back`">Nice Try !</h2>
    <h2 v-show="status == `back`">Your points : {{ point }}</h2>
    <br />
    <button v-show="status == `back`" class="btn btn-warning btn-lg active" @click="reset">Back to home</button>
    <!-- ------ -->
</div>

</template>

<style>
/* @import url("https://cdn.jsdelivr.net/npm/bootstrap-icons@1.3.0/font/bootstrap-icons.css%22);  */

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

#play-animation-easy {
    animation: progress-animation 5s forwards;
    size: 1ch;
    background-color: green;
}

#play-animation-normal {
    animation: progress-animation 2.5s forwards;
    size: 1ch;
    background-color: yellow;
}

#play-animation-hard {
    animation: progress-animation 1s forwards;
    size: 1ch;
    background-color: red;
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