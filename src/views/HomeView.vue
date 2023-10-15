<template>
  <div class="home">
    <!-- Questions -->
    <div class="questions" v-if="currentState === 'question'">
      <img class="logo" alt="Vue logo" src="../assets/logo.png">
      <div><input type="text" v-model="search" placeholder="မေးခွန်းများ ရှာရန်"></div>
      <div v-for="question in filteredQuestions" :key="question.questionNo">
        <SingleQuestion :question="question" @clicked="clickQuestion" />
      </div>
    </div>
    <!-- Question -->
    <div v-if="currentState === 'numbers' || currentState === 'result'">
      <h4>{{ this.currentQuestion.questionName }}</h4>
    </div>
    <!-- Numbers -->
    <div v-if="currentState === 'numbers'">
      <ChooseAnswer @clicked="clickAnswer" />
    </div>
    <!-- Show Result -->
    <div class="resultView" v-if="currentState === 'result'">
      <h4 class="result">{{ result.answerResult }}</h4>
      <button @click="restart()">ထပ်မေးမယ်</button>
    </div>
  </div>
</template>

<script>
import ChooseAnswer from '../components/chooseAnswer'
import SingleQuestion from '../components/singleQuestion'

export default {
  name: 'HomeView',
  components: {
    ChooseAnswer,
    SingleQuestion,
  },
  data() {
    return {
      apiQuestions: "http://localhost:3000/questions",
      apiAnswers: "http://localhost:3000/answers",
      search: "",
      questions: [],
      answers: [],
      currentState: "question",
      currentQuestion: 0,
      currentAnswerNumber: 0,
      result: {},
    }
  },
  computed: {
    filteredQuestions() {
      return this.questions.filter((q) => {
        return q.questionName.includes(this.search);
      })
    }
  },
  mounted() {
    // fetch Questions
    fetch(this.apiQuestions)
      .then((response) => {
        return response.json()
      })
      .then((datas) => {
        this.questions = datas
      })
      .catch((err) => {
        console.log(err)
      })

    // fetch Answers
    fetch(this.apiAnswers)
      .then((response) => {
        return response.json()
      })
      .then((datas) => {
        this.answers = datas
      })
      .catch((err) => {
        console.log(err)
      })
  },
  methods: {
    clickQuestion(question) {
      this.currentQuestion = question;
      this.currentState = "numbers";
    },
    clickAnswer(answerNumber) {
      this.currentAnswerNumber = answerNumber;
      this.result = this.answers.find((ans) => {
        return ans.questionNo == this.currentQuestion.questionNo
          && ans.answerNo == this.currentAnswerNumber
      });
      this.currentState = "result";
    },
    restart() {
      this.currentState = "question";
      this.currentQuestion = 0;
      this.currentAnswerNumber = 0;
      this.result = {};
    }
  }
}
</script>

<style>
button {
  display: block;
  margin: 20px auto 0;
  background-color: #f2f2f2;
  color: #666666;
  padding: 10px;
  border: 0;
  font-weight: bold;
  border-radius: 6px;
  font-size: 16px;
}

button:hover {
  cursor: pointer;
  color: #f2f2f2;
  background-color: #666666;
}

input {
  margin: 20px;
  padding: 10px;
  padding-left: 20px;
  border: 0;
  background-color: #dcdcdc;
  border-bottom: 2px solid #dcdcdc;
  width: 50%;
  box-sizing: border-box;
  border-radius: 10px;
}

.logo {
  width: 150px;
  height: 100%;
}

.resultView {
  align-items: center;
  height: auto;

}
.result{
  margin: 50px;
  padding: 30px;
  color: crimson;
  background-color: #f2f2f2;
  box-shadow: 0 4px 8px 0 rgba(0,0,0,0.2);
  transition: 0.3s;
  border-radius: 5px; 
}
.questions {
  align-items: center;
}
</style>