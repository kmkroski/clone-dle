<template>
  <div>
    <div
      v-for="row in 6" :key="row"
      class="row"
    >
      <div
        v-for="col in 5" :key="col"
        :class="{
          'cell': true,
          'active': isActive(row, col),
          'correct': checkResult(row, col, 'c'),
          'incorrect': checkResult(row, col, 'i'),
          'wrong': checkResult(row, col, 'w'),
        }"
      >
        {{ getLetter(row, col) }}
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: 'Board',
  props: {
    target: String,
    guesses: Array,
    results: Array,
  },
  methods: {
    getLetter(row, col) {
      return this.guesses[row - 1] ? this.guesses[row - 1].charAt(col - 1) : ''
    },
    isActive(row, col) {
      return this.guesses[row - 1] !== undefined ? col == this.guesses[row - 1].length + 1 : false
    },
    checkResult(row, col, check) {
      return this.results[row - 1] ? this.results[row - 1].charAt(col - 1) === check : false
    }
  }
}
</script>

<style scoped>
.row {
  display: flex;
  justify-content: space-around;
}

.cell {
  width: 4rem;
  height: 4rem;
  margin: 0.25rem;
  border: 2px solid #bdc3c7;
  font-size: 2rem;
  line-height: 4rem;
  text-align: center;
  font-weight: bold;
  text-transform: uppercase;
  border-radius: 0.25rem;
}

.cell.active {
  border-color: #2c3e50;
}
.cell.correct {
  color: #ecf0f1;
  background-color: #27ae60;
  border-color: #2ecc71;
}
.cell.incorrect {
  color: #ecf0f1;
  background-color: #f39c12;
  border-color: #f1c40f;
}
.cell.wrong {
  color: #ecf0f1;
  background-color: #2c3e50;
  border-color: #34495e;
}
</style>
