<template>
    <div class="questions-ctr">
      <div class="progress">
        <div class="bar" :style="{width: `${Object.keys(questionsAnswered).length / questions.length * 100}%`}"></div>
        <div class="status">{{ Object.keys(questionsAnswered).length }} out of {{ questions.length }} questions answered</div>
      </div>
      <transition-group name="fade">
            <div class="single-question" 
                v-for="(question,index) in questions" :key="question.q" 
                :class="{'backdrop-attempted': questionsAnswered.hasOwnProperty(index)}"
                v-show="currentIndex === index"
            >
                <div class="question">{{ index + 1 + '. '}}{{question.q}}</div>
                <div class="answers" v-for="answer in question.answers" :key="answer.text">
                    <div class="answer" 
                        @click="markAnswer(index,answer)" 
                        :class="[
                                questionsAnswered.hasOwnProperty(index) && questionsAnswered[index] === answer.text ? 'is-answered' : '',
                                questionsAnswered.hasOwnProperty(index) && answer['is_correct'] ? 'is-correct-answered' : '',
                            ]"
                    > 
                        <span>{{ answer.text }}</span>
                        <span v-if="questionsAnswered.hasOwnProperty(index) && answer['is_correct'] && questionsAnswered[index] === answer.text">
                            <input type="checkbox" :checked="true">
                        </span>
                        <span v-else-if="questionsAnswered.hasOwnProperty(index) && questionsAnswered[index] === answer.text">
                            <span class="wrong-answer-tick"> &cross; </span>
                        </span>
                    </div>
                </div>
            </div>
        </transition-group>
    </div>
</template>
<script>
export default {
    name: 'Questions',
    props: ['questions','questionsAnswered','currentIndex', 'correctAnswered'],
    methods: {
        markAnswer(index,answer) {
            this.$emit('submitAnswer', index, answer);
        }
    }
}
</script>
<style>
    
</style>