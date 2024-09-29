<script setup>
import {computed, ref} from "vue";

const alphabets = ref([]);
const startingCharCode = 97; //"a".charCodeAt() = 97
const endingCharCode = 122;
const currentWord = ref('');

for (let i = startingCharCode; i <= endingCharCode; i++) {
  const currentLetter = String.fromCharCode(i);
  alphabets.value.push({letter: currentLetter, selected: false});
}

const selectedLetters = computed(() => {
  return alphabets.value
      .filter(letter => letter.selected)
      .map(charData => charData.letter)
});

const searchOutput = computed(() => {
  let result = ''
  for (let char of currentWord.value.word) {
    if (selectedLetters.value.includes(char)) {
      result = result + char;
    } else {
      result = result + "_";
    }
  }
  return result;
});

const gameWon = computed(() => {
  const correctWord  = currentWord.value.word;
  return correctWord.split('').every(char =>
      selectedLetters.value.includes(char)
  );
});

const incorrectGuesses = computed(() => {
  let incorrectCount = 0;
  let selectedLettersArray = selectedLetters.value;

  for (let letter of selectedLettersArray) {
    if (!currentWord.value.word.includes(letter)) {
      incorrectCount++;
    }
  }
  return incorrectCount;
})


const wordList = ref([
  {word: 'pen', hint: 'Du schreibst damit'},
  {word: 'oxygen', hint: 'A gas essential for human respiration'},
  {word: 'telescope', hint: 'A tool used for viewing distant objects, especially in astronomy'},
  {word: 'guitar', hint: 'A stringed musical instrument often used in rock bands'},
]);

const selectRandomWord = () => {
  const index = Math.floor(Math.random() * wordList.value.length);
  currentWord.value = wordList.value[index];
  return currentWord.value
}
selectRandomWord();

const handleLetterClick = (charData) => {
  charData.selected = true;
};
const resetAllSelectedChars = () => {
  for (let letter of alphabets.value) {
    letter.selected = false;
  }
  return alphabets.value
}

const restartGame = () => {
  selectRandomWord();
  resetAllSelectedChars();

  console.log(alphabets.value);
}

</script>

<template>

  <div v-if="gameWon" class="game-over">
    <div class="game-over-content">
      <h1>Congratulations, You Won! 🎉</h1>
      <button @click="restartGame" class="new-game-btn">Play Again</button>
    </div>
  </div>

  <div v-if="incorrectGuesses>=10" class="game-over">
    <div class="game-over-content">
      <h1>Game Over</h1>
      <p>Try again!</p>
      <button @click="restartGame" class="restart-btn">Restart</button>
    </div>
  </div>

  <div>
    <div v-if="currentWord" class=" container playing-section">
      <div class="hint-box">{{ currentWord.hint }}</div>
      <div>{{ searchOutput }}</div>
    </div>
    <div class="container">
      <button
          v-for="alphabet in alphabets"
          :key="alphabet"
          @click="handleLetterClick(alphabet)"
          :disabled="alphabet.selected"
      >
        {{ alphabet.letter }}

      </button>
    </div>
  </div>

  <div class="hangman">
    <div class="gallows">
      <div class="post" :class="{'hangman-visible' : incorrectGuesses >=1}"></div>
      <div class="beam" :class="{'hangman-visible' : incorrectGuesses >=2}"></div>
      <div class="support" :class="{'hangman-visible' : incorrectGuesses >=3}"></div>
      <div class="rope" :class="{'hangman-visible' : incorrectGuesses >=4}"></div>
    </div>
    <div class="man">
      <div class="head" :class="{'hangman-visible' : incorrectGuesses >=5}"></div>
      <div class="body" :class="{'hangman-visible' : incorrectGuesses >=6}"></div>
      <div class="arm left-arm" :class="{'hangman-visible' : incorrectGuesses >=7}"></div>
      <div class="arm right-arm" :class="{'hangman-visible' : incorrectGuesses >= 8}"></div>
      <div class="leg left-leg" :class="{'hangman-visible' : incorrectGuesses >=9}"></div>
      <div class="leg right-leg" :class="{'hangman-visible' : incorrectGuesses >=10}"></div>
    </div>
  </div>

</template>

<style scoped>
.container {
  display: flex;
  flex-direction: row;
  gap: 10px;
}

.button-wrapper {
  border: 1px solid rgba(0, 0, 0, 1);
  border-radius: 5px;
  margin: 5px;
  padding: 4px;
  background-color: rgba(0, 0, 0, 0.2);
}

.button-wrapper:hover {
  background-color: rgba(0, 0, 0, 0.5)
}

.hint-box {
  border: 1px solid rgba(0, 0, 0, 1);
  border-radius: 5px;
  margin: 5px;
  width: 35%;
  padding: 4px;
  background-color: rgba(0, 0, 0, 0.2);
}

.hangman {
  position: relative;
}

.gallows {
  position: relative;
}

.post {
  width: 10px;
  height: 500px;
  background-color: #333;
  position: absolute;
  left: 50px;
  opacity: 0.5
}

.beam {
  width: 150px;
  height: 10px;
  background-color: #333;
  position: absolute;
  top: 0;
  left: 50px;
  border-radius: 5px;
  opacity: 0.5
}

.support {
  height: 85px;
  width: 4px;
  position: absolute;
  transform: rotate(30deg);
  background-color: #333;
  left: 100px;
  transform-origin: top center;
  opacity: 0.5
}

.rope {
  width: 2px;
  height: 50px;
  background-color: #333;
  position: absolute;
  top: 5px;
  left: 198px;
  opacity: 0.5
}

.man {
  position: absolute;
  top: 55px;
  left: 180px;
}

.head {
  width: 40px;
  height: 40px;
  border-radius: 50%;
  background-color: #333;
  position: absolute;
  top: 0;
  left: 0;
  opacity: 0.5;
  z-index: 100;
}

.body {
  position: absolute;
  top: 10px;
  height: 100px;
  width: 4px;
  left: 18px;
  background-color: #333;
  opacity: 0.5
}

.arm {
  position: absolute;
  top: 50px;
  height: 50px;
  width: 4px;
  left: 18px;
  background-color: #333;
  transform-origin: left top;
  opacity: 0.5
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
  opacity: 0.5
}

.left-leg {
  transform: rotate(45deg);
}

.right-leg {
  transform: rotate(-45deg);
}

.hangman-visible {
  opacity: 1;
}


.game-over {
  position: fixed; /* Position it relative to the viewport */
  top: 0;
  left: 0;
  width: 100vw;
  height: 100vh;
  background-color: rgba(0, 0, 0, 0.8); /* Dark transparent overlay */
  display: flex;
  justify-content: center; /* Center horizontally */
  align-items: center; /* Center vertically */
  z-index: 9999; /* Ensure it appears on top of other elements */
}

/* Content inside the game over div */
.game-over-content {
  text-align: center;
  color: white; /* Text color */
  background-color: #222; /* Slightly lighter background for the content */
  padding: 40px;
  border-radius: 10px;
  box-shadow: 0 4px 10px rgba(0, 0, 0, 0.5); /* Subtle shadow for depth */
  font-family: 'Arial', sans-serif;
}

/* Styling for the Game Over heading */
.game-over-content h1 {
  font-size: 48px;
  margin-bottom: 20px;
  letter-spacing: 2px;
}

/* Paragraph text */
.game-over-content p {
  font-size: 18px;
  margin-bottom: 20px;
}

/* Button styling */
.restart-btn {
  padding: 10px 20px;
  font-size: 18px;
  background-color: #f44336; /* Red button */
  color: white;
  border: none;
  border-radius: 5px;
  cursor: pointer;
  transition: background-color 0.3s ease;
}

.restart-btn:hover {
  background-color: #d32f2f; /* Darker red on hover */
}

.new-game-btn {
  padding: 10px 20px;
  font-size: 18px;
  background-color: #00ff00; /* Red button */
  border: none;
  border-radius: 5px;
  cursor: pointer;
  transition: background-color 0.3s ease;
}


</style>