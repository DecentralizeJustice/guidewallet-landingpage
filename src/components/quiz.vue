<template>
  <v-row align="center" justify='center' no-gutters>
    <v-col cols='10' md='8' class="text-center mt-2">
      <v-alert type="error" v-if='wrong'>
        Sorry, your answer is wrong. Try Again.
      </v-alert>
      <v-alert type="success" v-if='done' class="text-h6">
        Part 1 Done 😎  Rest of Lesson Is in Wallet.
      </v-alert>
    </v-col>
    <v-col cols='6' v-if='!done'>
      <v-progress-linear
        :value='progress'
        color="green"
        height="25"
      >
        <template v-slot="{ value }">
          <strong>{{ Math.ceil(value) }}%</strong>
        </template>
      </v-progress-linear>
    </v-col>
    <v-col class="mt-3" cols="11" md='10' v-if='!done'>
      <v-card class="mx-auto" color='blue darken-4'
        >
      <v-card-text class="display-1 white--text">
      {{question}}
      </v-card-text>
    </v-card>
    </v-col>
    <v-col cols='12'>
    <v-list rounded v-if='!done'>
      <v-list-item-group
      color='primary'
      v-model="chosen"
      >
        <v-list-item
          v-for="(item, i) in choices"
          :key="i"
          color='primary'
          @click='select(i)'
        >
          <v-list-item-content >
            <v-list-item-title v-text="item.value"
            class="text-h6"></v-list-item-title>
          </v-list-item-content>
        </v-list-item>
      </v-list-item-group>
    </v-list>
  </v-col>
      <v-col cols='12' class="text-center mb-3">
        <v-btn
          color="red darken-1"
          @click="backToVideo()"
        >
          Back To Video
        </v-btn>
      </v-col>
    </v-row>
</template>

<script>
export default {
  name: 'quizVideoPreview',
  components: {
  },
  props: ['questions'],
  data () {
    return {
      questionNum: 0,
      wrong: false,
      done: false,
      chosen: null
    }
  },
  mounted () {
    // console.log()
    // console.log(allLessons.introBTC)
    // console.log(this.lesson)
  },
  methods: {
    backToVideo () {
      this.$emit('backToVideo')
    },
    checkAnswer () {
      if (this.chosen === this.correctAnswer) {
        this.correct()
      } else {
        this.wrong = true
      }
    },
    select (i) {
      this.chosen = i
      setTimeout(() => {
        this.checkAnswer()
      }, 50)
    },
    correct () {
      this.chosen = undefined
      this.wrong = false
      if (this.questionNum !== this.questions.length - 1) {
        this.questionNum += 1
      } else {
        this.done = true
      }
    }
  },
  computed: {
    choices: function () {
      const letters = ['A. ', 'B. ', 'C. ', 'D. ', 'E. ', 'F. ', 'G. ']
      const options = []
      const quest = this.questions
      for (let i = 0; i < quest[this.questionNum].opt.length; i++) {
        options.push(
          {
            value: letters[i].concat(quest[this.questionNum].opt[i]),
            index: i
          }
        )
      }
      return options
    },
    question: function () {
      const value = this.questions[this.questionNum].q
      return value
    },
    correctAnswer: function () {
      const value = this.questions[this.questionNum].ans
      return value
    },
    progress: function () {
      const numberOfQuestions = this.questions.length
      return (this.questionNum / numberOfQuestions) * 100
    }
  }
}
</script>
