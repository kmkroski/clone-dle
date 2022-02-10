<template>
  <h3>Uh-oh!</h3>

  <p><strong>{{ word.toUpperCase() }}</strong> is not a word!</p>
</template>

<script>
export default {
  name: 'ModalInvalid',
  props: {
    word: String,
  }
}
</script>
