<template>
  <div class="card">
    <div class="user">
      <div class="user__inner">
        <p class="user-name">Anton</p>
        <div class="user-coins">
          <span>1000</span>
          <img src="../assets/coin.png" alt="coins" />
        </div>
      </div>
    </div>
    <div class="card__question">
      <button class="close"></button>
      <div class="card__question-task">
        <h2>{{ state.task }}</h2>
        <p class="task">{{ state.question }}</p>
      </div>
      <button id="show-modal" @click="showModal = true" class="hint"></button>
    </div>
  </div>

  <img :src="state.imagePath" alt="" />
  <div class="buttons">
    <RadioButton
      v-if="state.items"
      :options="state.items"
      v-model:selectedOption="state.value"
      @change="handleChange"
    ></RadioButton>
    <button class="result" @click="checkAnswer">CHECK</button>
  </div>

    <Teleport to="body">
    <!-- use the modal component, pass in the prop -->
    <modal :show="showModal" @close="showModal = false">
      <template #header>
        <h3>custom header</h3>
      </template>
    </modal>
  </Teleport>
</template>

<style scoped></style>

<script setup>
import { reactive, onMounted, ref } from "vue";
import RadioButton from "./RadioButton.vue";
import Modal from './Modal.vue'
const showModal = ref(false)

const handleChange = ($event) => {
  state.value = $event.target.value;
  console.log($event.target.value, "qq");
};

const state = reactive({
  items: [],
  imagePath: "",
  value: "",
  task: "",
  correctAnswer: "",
  question: "",
});

const checkAnswer = () => {
  if (state.value === state.correctAnswer) {
    alert("Correct!");
  } else {
    alert("Wrong answer.");
  }

  reset();
};

const reset = () => {
  state.value = "";
};

onMounted(async () => {
  try {
    const response = await fetch("./src/assets/data.json");
    const data = await response.json();
    console.log(data);
    state.items = data.options;
    state.imagePath = data.image;
    state.task = data.task;
    state.correctAnswer = data.correct_answer;
    state.question = data.question;
    console.log(state.imagePath);
  } catch (error) {
    console.error("Error fetching data:", error);
  }
});
</script>

<style>
.result {
  text-align: center;
  padding: 10px;
  width: 15%;
  cursor: pointer;
  border: none;
  background: rgba(255, 255, 255, 0.5);
  border-radius: 6px;
  flex-shrink: 0;
  -webkit-box-shadow: 0px 16px 5px -7px rgba(34, 60, 80, 0.33);
  -moz-box-shadow: 0px 16px 5px -7px rgba(34, 60, 80, 0.33);
  box-shadow: 0px 16px 5px -7px rgba(34, 60, 80, 0.33);
}
.card {
  text-align: center;
}
.card h2 {
  font-size: 12px;
}
.task {
  font-size: 20px;
  text-align: center;
  margin-bottom: 20px;
}
.buttons {
  display: flex;
  width: 95%;
  left: 50%;
  transform: translate(-50%, 0);
  background: rgba(0, 0, 0, 0.3);
  padding: 15px;
  gap: 20px;
  position: absolute;
  bottom: 10px;
}

.user {
  display: flex;
  justify-content: end;
}
.user__inner {
  display: flex;
  flex-direction: column;
  align-items: center;
}

.user-coins {
  display: flex;
  align-items: center;
  gap: 5px;
}

.card__question {
  display: flex;
  justify-content:space-between;
  align-items: center;
}

.close, .hint {
  width: 30px;
  height: 30px;
  border-radius: 6px;
  border: none;
    -webkit-box-shadow: 0px 16px 5px -7px rgba(34, 60, 80, 0.33);
  -moz-box-shadow: 0px 16px 5px -7px rgba(34, 60, 80, 0.33);
  box-shadow: 0px 16px 5px -7px rgba(34, 60, 80, 0.33);
}
.close {
  background: url(../assets/close_btn.png) no-repeat center;
  background-size: 20px;
}
.hint {
  background: url(../assets/bulb.png) no-repeat center;
   background-size: 20px;
}


@media (max-width: 680px) {
  .buttons {
    flex-direction: column;
  }
}
@media (max-width: 580px) {
  .result {
    width: 100%;
  }
}
</style>
