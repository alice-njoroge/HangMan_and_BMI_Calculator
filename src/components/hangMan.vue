<script setup>
import {ref} from "vue";

const alphabets = ref([]);
const startingCharCode = 97; //"a".charCodeAt() = 97
const endingCharCode = 122

for (let i = startingCharCode; i < endingCharCode; i++) {
  const currentLetter = String.fromCharCode(i);
  alphabets.value.push(currentLetter);
}


</script>

<template>
  <div v-for="alphabet in alphabets">
  {{alphabet}}
  </div>

  <div class="hangman">
    <div class="gallows">
      <div class="post"></div>
      <div class="beam"></div>
      <div class="support"></div>
      <div class="rope"></div>
    </div>
    <div class="man">
      <div class="head"></div>
      <div class="body"></div>
      <div class="arm left-arm"></div>
      <div class="arm right-arm"></div>
      <div class="leg left-leg"></div>
      <div class="leg right-leg"></div>
    </div>
  </div>

</template>

<style scoped>
.hangman {
  position: relative;
}

.gallows {
  position: relative;
  opacity: 0.5;
}

.post {
  width: 10px;
  height: 500px;
  background-color: #333;
  position: absolute;
  left: 50px;

}
.beam {
  width: 150px;
  height: 10px;
  background-color: #333;
  position: absolute;
  top: 0;
  left: 50px;
  border-radius: 5px;
}
.support {
  height: 85px;
  width: 4px;
  position: absolute;
  transform: rotate(30deg);
  background-color: #333;
  left: 100px;
  transform-origin: top center;
}

.rope {
  width: 2px;
  height: 50px;
  background-color: #333;
  position: absolute;
  top: 5px;
  left: 198px;

}
.man {
  position: absolute;
  top: 55px;
  left: 180px;
  opacity: 0.5;
}
.head {
  width: 40px;
  height: 40px;
  border-radius: 50%;
  background-color: #333;
  position: absolute;
  top: 0;
  left: 0;
}
.body {
  position: absolute;
  top: 10px;
  height: 100px;
  width: 4px;
  left: 18px;
  background-color: #333;
}
.arm {
  position: absolute;
  top: 50px;
  height: 50px;
  width: 4px;
  left: 18px;
  background-color: #333;
  transform-origin: left top;
}
.left-arm {
  transform: rotate(45deg);
}
.right-arm {
  transform: rotate(-45deg);
}
.leg {
  position: absolute;
  top: 108px;
  height: 70px;
  width: 4px;
  left: 18px;
  background-color: #333;
  transform-origin: left top;
}

.left-leg{
  transform: rotate(45deg);
}
.right-leg{
  transform: rotate(-45deg);
}



</style>