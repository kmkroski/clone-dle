<template>
  <Header />
  <Board :target="target" :guesses="guesses" :results="results" />
  <Keyboard :keyStatus="keyStatus" @keydown="handleKeydown" />

  <ModalContainer v-if="state !== 'play'" @close="closeModal">
    <ModalCorrect v-if="state === 'win'" :target="target" />
    <ModalInvalid v-if="state === 'invalid'" :word="guesses[guesses.length - 1]" />
    <ModalWrong v-if="state === 'lose'" :target="target" />
  </ModalContainer>
</template>

<script>
import words from '@/assets/words.js'

import Board from '@/components/Board.vue'
import Header from '@/components/Header.vue'
import Keyboard from '@/components/Keyboard.vue'

import ModalContainer from '@/components/modals/ModalContainer.vue'
import ModalCorrect from '@/components/modals/ModalCorrect.vue'
import ModalInvalid from '@/components/modals/ModalInvalid.vue'
import ModalWrong from '@/components/modals/ModalWrong.vue'

export default {
  name: 'App',
  data: () => ({
    state: '',
    guesses: [],
    target: '',
    results: [],
    keyStatus: {},
  }),
  components: {
    Board,
    Header,
    Keyboard,
    ModalContainer,
    ModalCorrect,
    ModalInvalid,
    ModalWrong,
  },
  mounted() {
    window.addEventListener('keydown', this.handleKeydown)

    this.setupGame()
  },
  methods: {
    setupGame() {
      this.target = 'hello' //words[Math.floor(Math.random() * words.length)]
      this.guesses = ['']
      this.results = new Array(5).fill('-----')
      this.keyStatus = {
        'c': '',
        'i': '',
        'w': '',
      }

      this.state = 'play'
    },
    handleKeydown(key) {
      if (this.state !== 'play') {
        return
      }

      if (key.key) {
        key = key.key.toLowerCase()
      }

      if (key === 'backspace') {
        this.guesses[this.guesses.length - 1] = this.guesses[this.guesses.length - 1].slice(0, -1)
        return
      }

      if (key === 'enter' && this.guesses[this.guesses.length - 1].length == 5) {
        this.checkRow(this.guesses.length - 1)
        return
      }

      if (this.guesses[this.guesses.length - 1].length == 5) {
        return
      }

      if ('abcdefghijklmnopqrstuvwxyz'.split('').indexOf(key) == -1) {
        return
      }

      this.guesses[this.guesses.length - 1] += key
    },
    checkRow(row) {
      if (words.indexOf(this.guesses[row]) == -1) {
        this.state = 'invalid'
        return
      }

      let guess = this.guesses[row].split('')
      let word = this.target.split('')
      let result = 'wwwww'.split('')

      for (let i = 0; i < 5; i++) {
        if (guess[i] === word[i]) {
          this.logValidKeys(guess[i], 'c')

          word[i] = '-'
          guess[i] = '-'
          result[i] = 'c'
        }
      }

      for (let i = 0; i < 5; i++) {
        if (word.indexOf(guess[i]) !== -1 && guess[i] !== '-') {
          this.logValidKeys(guess[i], 'i')

          word[word.indexOf(guess[i])] = '-'
          guess[i] = '-'
          result[i] = 'i'
        } else {
          this.logValidKeys(guess[i], 'w')
        }
      }

      this.results[row] = result.join('')
      if (this.results[row] === 'ccccc') {
        this.state = 'win'
      } else if (row === 5) {
        this.state = 'lose'
      } else {
        this.guesses.push('')
      }
    },
    logValidKeys(key, type) {
      if (
        type === 'c' &&
        this.keyStatus['c'].indexOf(key) === -1
      ) {
        this.keyStatus['c'] += key
        this.keyStatus['i'] = this.keyStatus['i'].replace(key, '')
        this.keyStatus['w'] = this.keyStatus['w'].replace(key, '')
      } else if (
        type === 'i' &&
        this.keyStatus['c'].indexOf(key) === -1 &&
        this.keyStatus['i'].indexOf(key) === -1
      ) {
        this.keyStatus['i'] += key
        this.keyStatus['w'] = this.keyStatus['w'].replace(key, '')
      } else if (
        type === 'w' &&
        this.keyStatus['c'].indexOf(key) === -1 &&
        this.keyStatus['i'].indexOf(key) === -1 &&
        this.keyStatus['w'].indexOf(key) === -1
      ) {
        this.keyStatus['w'] += key
      }
    },
    closeModal() {
      if (this.state === 'invalid') {
        this.guesses[this.guesses.length - 1] = ''
        this.state = 'play'
      } else {
        this.setupGame()
      }
    },
  }
}
</script>

<style>
html, body {
  margin: 0;
  padding: 0;
  background-color: #ecf0f1;
  color: #2c3e50;
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
}

#app {
  display: flex;
  align-items: center;
  justify-content: space-between;
  flex-direction: column;
  min-height: 92vh;
  width: 100%;
  max-width: 600px;
  margin: 0 auto;
}
</style>
