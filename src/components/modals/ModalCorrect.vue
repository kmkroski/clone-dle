<template>
  <h3>Excellent!</h3>

  <p>You found the correct word!</p>

  <p>The word was: <strong>{{ target.toUpperCase() }}</strong>!</p>
</template>

<script>
export default {
  name: 'ModalCorrect',
  props: {
    target: String,
  }
}
</script>
