<script setup>
import { ref, computed } from "vue";

const questions = [
  {
    question: "What is the capital of France?",
    options: ["London", "Berlin", "Paris", "Rome"],
    answer: "Paris",
  },
  {
    question: "Which planet is closest to the sun?",
    options: ["Earth", "Mars", "Venus", "Mercury"],
    answer: "Mercury",
  },
  // Add more questions as needed
];

const currentQuestionIndex = ref(0);

// click next stuff
const isShowSubmitBtn = ref(true);
const onClickNext = () => {
  currentQuestionIndex.value += 1;
  isShowSubmitBtn.value = true;
  isClickSubmit.value = false;
  selectedOption.value = "";
};

// click submit stuff
const isClickSubmit = ref(false);
const onClickSubmit = () => {
  isShowSubmitBtn.value = false;
  isClickSubmit.value = true;

  if (isUserCorrect.value) {
    scoreCount.value += 1;
  }
};

// click option stuff
const selectedOption = ref("");
const correctAnswer = ref("");
const isUserCorrect = computed(() => {
  return selectedOption.value === correctAnswer.value;
});
const onClickOption = (option, answer) => {
  selectedOption.value = option;
  correctAnswer.value = answer;
};

const scoreCount = ref(0);

const redo = () => {
  currentQuestionIndex.value = 0;
  selectedOption.value = "";
  correctAnswer.value = "";
  scoreCount.value = 0;
};
</script>

<template>
  <div class="text-white w-1/3 h-1/3">
    <div v-if="currentQuestionIndex <= questions.length - 1">
      <div v-for="(item, index) in questions" :key="JSON.stringify(item)">
        <div v-show="currentQuestionIndex === index">
          <h1 class="border-b text-center inline-block p-2">
            {{ item.question }}
          </h1>
          <ul>
            <li
              v-for="option in item.options"
              :key="JSON.stringify(option)"
              class="p-2 flex items-center cursor-pointer"
              @click="onClickOption(option, item.answer)"
            >
              <div
                class="w-2 h-2 mr-3 rounded-full bg-white"
                :class="{ '!bg-blue-700': selectedOption === option }"
              ></div>
              <div>
                {{ option }}
              </div>
            </li>
          </ul>
        </div>
      </div>
      <div class="flex justify-between gap-x-5">
        <button
          v-if="isShowSubmitBtn"
          class="p-1 w-20 rounded bg-blue-500"
          :class="{ 'bg-gray-500': selectedOption === '' }"
          :disabled="selectedOption === ''"
          @click="onClickSubmit"
        >
          Submit
        </button>
        <button
          v-else
          class="p-1 w-20 rounded bg-blue-500"
          @click="onClickNext"
        >
          Next
        </button>
        <div
          v-show="!isUserCorrect && isClickSubmit"
          class="px-1 text-center border-2 border-red-800 text-red-800 flex-1"
        >
          Incorrect
        </div>
        <div
          v-show="isUserCorrect && isClickSubmit"
          class="px-1 text-center border-2 border-green-800 text-green-800 flex-1"
        >
          Correct
        </div>
      </div>
    </div>

    <div v-else>
      <div>
        your final score: {{ scoreCount }} out of {{ questions.length }}
      </div>

      <div class="mt-5">
        <button class="p-1 w-20 rounded bg-blue-500" @click="redo">redo</button>
      </div>
    </div>
  </div>
</template>
