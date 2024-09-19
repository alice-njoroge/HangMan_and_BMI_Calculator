<script setup>
import {computed, ref} from "vue";

//transition increase the numbers from 0 to current number and have a bounce/color effect in the end

const form = ref({
  height: 0,
  weight: 0
});
const touched = ref({
  height: false,
  weight: false
})
const bmi = ref(0);

const errorHeight = computed(() => {
  const maxHeight = 300;
  return touched.value.height && (form.value.height <= 0 || form.value.height > maxHeight)
});
const errorWeight = computed(() => {
  const maxWeight = 650;
  return touched.value.weight && (form.value.weight <= 0 || form.value.weight > maxWeight)
});
const showComment = ref(false);

const calculateBMI = () => {
  if (errorWeight.value && errorHeight.value) {
    alert('message')
    return ;
  }
  showComment.value = true;
  // BMI = weight in KG / (height in m * height in m)
  const heightInMetres = form.value.height / 100;
  bmi.value = form.value.weight / (heightInMetres * heightInMetres)
  return bmi.value;
}

const comment = computed(() => {
  const status = ref('');
  if (bmi.value < 18.5) {
    status.value = "Underweight"
  }
  if (bmi.value >= 18.5 && bmi.value < 25) {
    status.value = "Healthy"
  }
  if (bmi.value >= 25 && bmi.value < 30) {
    status.value = "Overweight"
  }
  if (bmi.value >= 30) {
    status.value = "Obese"
  }
  return status;
})

</script>

<template>
  <div class="container">
    <div class="box">
      {{ form }}
      <h1>BMI Calculator</h1>
      <div class="content">
        <div class="input">
          <label for="height">Height(cm)</label>
          <input v-model.number.lazy="form.height" @blur="touched.height = true" type="number" id="height">
          <div style="color: #d12322" v-if="errorHeight"> Valid Height is required</div>
        </div>
        <div class="input">
          <label for="weight">Weight(kg)</label>
          <input v-model.number="form.weight" @blur="touched.weight = true" type="number" id="weight">
          <div style="color: #d12322" v-if="errorWeight"> Valid Height is required</div>
        </div>
        <button @click="calculateBMI" id="calculate">Calculate BMI</button>
      </div>
      <div class="result">
        <p>Your BMI is</p>
        <div id="result">{{ bmi.toFixed(2) }}</div>
        <p v-if="showComment" class="comment">Comment: You are {{ comment }}</p>
      </div>
    </div>
  </div>
  <a href="https://youtu.be/sm4QVfhOP6Y" target="_blank" class="link">Watch on youtube <i
      class="fab fa-youtube"></i></a>

</template>

<style scoped>
@import url("https://fonts.googleapis.com/css2?family=Nunito:wght@600;700;800;900&display=swap");

* {
  padding: 0;
  margin: 0;
  box-sizing: border-box;
}

body {
  min-height: 100vh;
  background: url("https://digitshack.com/codepen/bmical/bg.png");
  background-size: cover;
  background-repeat: no-repeat;
  font-family: "Nunito", sans-serif;
}

.container {
  position: absolute;
  left: 50%;
  top: 50%;
  transform: translate(-50%, 0%);
}

.box {
  min-width: 400px;
  background: #fafafa;
  border-radius: 38px;
  text-align: center;
  position: relative;
  margin-bottom: 150px;
}

.box::before {
  content: "";
  position: absolute;
  height: 110%;
  width: 110%;
  left: -5%;
  top: -5%;
  background-color: rgba(255, 255, 255, 0.3);
  box-shadow: 0px 0px 166px -31px rgba(0, 0, 0, 0.15);
  border-radius: 60px;
  z-index: -1;
}

h1 {
  font-weight: bold;
  font-size: 36px;
  padding: 30px 0;
}

.content {
  padding: 0 40px;
}

.input {
  background: #fff;
  box-shadow: 0px 0px 95px -30px rgba(0, 0, 0, 0.15);
  border-radius: 28px;
  padding: 20px 0;
  margin-bottom: 20px;
}

.input label {
  display: block;
  font-size: 18px;
  font-weight: 600;
  color: #000;
  margin-bottom: 20px;
}

.input input {
  outline: none;
  border: none;
  border-bottom: 1px solid #4f7df9;
  width: 60%;
  text-align: center;
  font-size: 28px;
  font-family: "Nunito", sans-serif;
}

button#calculate {
  font-family: "Nunito", sans-serif;
  color: #fff;
  background: #4f7df9;
  font-size: 16px;
  border-radius: 12px;
  padding: 12px 0;
  width: 100%;
  outline: none;
  border: none;
  transition: background 0.2s ease;
}

button#calculate:hover {
  background: #0044ff;
}

.result {
  padding: 30px 20px;
}

.result p {
  font-weight: 600;
  font-size: 22px;
  color: #000;
  margin-bottom: 15px;
}

.result #result {
  font-size: 36px;
  font-weight: 900;
  color: #4f7df9;
  background-color: #eaeaea;
  display: inline-block;
  padding: 7px 20px;
  border-radius: 55px;
  margin-bottom: 25px;
}

#comment {
  color: #4f7df9;
  font-weight: 800;
}

/* Youtube link */
.link {
  position: fixed;
  background-color: #d12322;
  padding: 23px 40px;
  right: -90px;
  border-radius: 5px;
  top: 50%;
  transform: translateY(-50%);
  transform: rotate(-90deg);
  font-size: 18px;
  font-weight: 500;
  color: #fff;
  text-decoration: none;
  text-transform: capitalize;
  transition: all 0.1s ease-in-out;
}

.link i {
  padding-left: 7px;
}

.link:hover {
  text-decoration: underline;
  background-color: black;
}


</style>