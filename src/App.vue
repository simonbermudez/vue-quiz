<template>
  <div id="app">
    <Header
    :numCorrect="numCorrect"
    :numTotal="numTotal"
    />
    <b-container class="bv-example-row">
      <b-row>
        <b-col sm="6" offset="3">
          <QuestionBox
            v-if="questions.length"
            :currentQuestion="questions[index]"
            :next="next"
            :increment="increment"
          />
        </b-col>
      </b-row>
    </b-container>
    
  </div>
</template>

<script>
import Header from './components/Header.vue'
import QuestionBox from './components/QuestionBox.vue'

export default {
  name: 'App',
  components: {
    Header,
    QuestionBox
  },
  data() {
    return {
      questions: [],
      index: 0,
      numCorrect: 0,
      numTotal: 0
    }
  },
  methods: {
    next() {
      this.index++
    },
    increment(isCorrect) {
      if (isCorrect) {
        this.numCorrect++
      }
      this.numTotal++
    }
  },
  mounted: function() {
    fetch('https://opentdb.com/api.php?amount=10&type=multiple', {
      method: 'get'
    })
      .then((response) => response.json())
      .then((jsonData) => this.questions = jsonData.results)
  }
}
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}

.list-group {
  margin-bottom: 15px !important;
}

.list-group-item:hover {
  background: #eee;
  cursor: pointer;
}

.btn {
  margin: 0 5px;
}

.selected {
  background-color: cyan !important;
}

.correct {
  background-color: green !important;
}

.incorrect {
  background-color: red !important;
}
</style>
