<template>
  <h3>Oh-no!</h3>

  <p>You weren't able to determine the correct word!</p>

  <p>The word was: <strong>{{ target.toUpperCase() }}</strong>!</p>
</template>

<script>
export default {
  name: 'ModalWrong',
  props: {
    target: String,
  }
}
</script>
