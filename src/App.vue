<template >
  <div class="ctr">
    <transition name="fade" mode="out-in">
      <Questions 
        v-if="!showResult"
        :questions="questions" 
        :questionsAnswered="questionsAnswered"
        :currentIndex="currentIndex"
        :correctAnswered="correctAnswered"
        @submitAnswer="onSubmit"
      />
      <Result
        v-else
        :questions="questions" 
        :questionsAnswered="questionsAnswered"
        :results="results"
        :correctAnswered="correctAnswered"
        @submitAnswer="onSubmit"
      />
    </transition>
    <div class="actions">
      <button type="button" v-if="!showResult" class="action-btn" @click="openPreviousQuestion" :disabled="currentIndex <= 0">Previous</button>
      <button type="button" v-if="showResult" class="reset-btn" @click="reset">Reset</button>
      <button type="button" v-if="!showResult" class="action-btn" @click="openNextQuestion" :disabled="currentIndex + 1 >= questions.length">Next</button>
    </div>
  </div>
</template>
<script>
import Result from './components/Result.vue';
import Questions from './components/Questions.vue';
export default {
  name:'App',
  components: {
    Result,
    Questions
  },
  data() {
    return {
      questions: [
        {
          q: 'What is 2 + 2?', 
          answers: [
            {
              text: '4',
              is_correct: true
            },
            {
              text: '3',
              is_correct: false 
            },
            {
              text: 'Fish',
              is_correct: false 
            },
            {
              text: '5',
              is_correct: false 
            }
          ] 
        },
        { 
          q: 'How many letters are in the word "Banana"?', 
          answers: [
            {
              text: '5',
              is_correct: false
            },
            {
              text: '7',
              is_correct: false 
            },
            {
              text: '6',
              is_correct: true 
            },
            {
              text: '12',
              is_correct: false 
            }
          ] 
        },
        { 
          q: 'Find the missing letter: C_ke', 
          answers: [
            {
              text: 'e',
              is_correct: false
            },
            {
              text: 'a',
              is_correct: true 
            },
            {
              text: 'i',
              is_correct: false 
            }
          ] 
        },
      ],
      results: [
        {
          min: 0,
          max: 2,
          title: "Try again!",
          desc: "Do a little more studying and you may succeed!"
        },
        {
          min: 3,
          max: 3,
          title: "Wow, you're a genius!",
          desc: "Studying has definitely paid off for you!"
        }
      ],
      questionsAnswered: {},
      correctAnswered: 0,
      currentIndex: 0,
    }
  },
  methods: {
    openNextQuestion() {
      if(this.currentIndex + 1 === this.questions.length) return;
      this.currentIndex++;
    },
    openPreviousQuestion() {
      if(this.currentIndex <= 0) return;
      this.currentIndex--;
    },
    reset() {
      this.questionsAnswered= {};
      this.correctAnswered= 0;
      this.currentIndex= 0;
    },
    onSubmit(index,answer) {
      if(this.questionsAnswered.hasOwnProperty(index)) return;
      if(answer.is_correct) this.correctAnswered++;
      this.questionsAnswered[index] = answer.text;
      this.openNextQuestion();
    }
  },
  computed: {
    showResult() {
      return Object.keys(this.questionsAnswered).length === this.questions.length;
    }
  }
}
</script>
<style>
  
</style>