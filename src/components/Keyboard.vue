<template>
  <div class="keyboard">
    <div
      v-for="row in keys" :key="row"
      class="keyboardRow"
    >
      <button
        v-for="key in row" :key="key"
        :class="{
          'keyboardKey': true,
          'correct':  keyStatus['c'] && keyStatus['c'].indexOf(key) !== -1,
          'incorrect': keyStatus['i'] && keyStatus['i'].indexOf(key) !== -1,
          'wrong': keyStatus['w'] && keyStatus['w'].indexOf(key) !== -1,
        }"
        @click="emitKeydown(key)"
      >
        <span v-if="key == '<'">&#8592;</span>
        <span v-else-if="key == '>'">&#9166;</span>
        <span v-else>{{ key }}</span>
      </button>
    </div>
  </div>
</template>

<script>
export default {
  name: 'Keyboard',
  props: {
    keyStatus: Object,
  },
  data: () => ({
    keys: [
      'qwertyuiop'.split(''),
      'asdfghjkl'.split(''),
      '<zxcvbnm>'.split(''),
    ]
  }),
  methods: {
    emitKeydown(key) {
      if (key == '<') {
        this.$emit('keydown', { key: 'Backspace' })
      } else if (key == '>') {
        this.$emit('keydown', { key: 'Enter' })
      } else {
        this.$emit('keydown', { key })
      }
    },
  }
}
</script>

<style>
.keyboardRow {
  display: flex;
  justify-content: center;
}

.keyboardKey {
  min-width: 2rem;
  height: 3rem;
  margin: 0.25rem;
  padding: 0.5rem;
  background-color: #2c3e50;
  color: #ecf0f1;
  border: none;
  border-radius: 0.25rem;
  font-weight: bold;
  text-align: center;
  text-transform: uppercase;
}
.keyboardKey:active {
  background-color: #34495e;
}
.keyboardKey:hover {
  opacity: 0.75;
}
.keyboardKey.correct {
  background-color: #27ae60;
}
.keyboardKey.incorrect {
  background-color: #f39c12;
}
.keyboardKey.wrong {
  background-color: #7f8c8d;
}
</style>
