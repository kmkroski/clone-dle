<template>
  <h3>Excellent!</h3>

  <p>You found the correct word!</p>

  <div>
    <div v-for="result in results" :key="result">
      {{ convertToEmoji(result) }}
    </div>
  </div>

  <p>The word was: <strong>{{ target.toUpperCase() }}</strong>!</p>
</template>

<script>
export default {
  name: 'ModalCorrect',
  props: {
    target: String,
    results: Array,
  },
  methods: {
    convertToEmoji(value) {
      return value
        .replace(/-/g, '')
        .replace(/c/g, '🟩')
        .replace(/i/g, '🟨')
        .replace(/w/g, '⬛')
    },
  },
}
</script>
