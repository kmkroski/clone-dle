<template>
  <div class="modalContainer">
    <div class="modal">
      <slot />
      <button class="modalButton" @click="$emit('close')">OK</button>
    </div>
  </div>
</template>

<style>
.modalContainer {
  position: absolute;
  top: 0; bottom: 0; left: 0; right: 0;
  display: flex;
  align-items: center;
  justify-content: center;
  background-color: rgba(0, 0, 0, 0.5);
}

.modal {
  width: 60%;
  max-width: 20rem;
  padding: 0 1rem;
  border-radius: 0.5rem;
  background-color: #ecf0f1;
  font-size: 1.25rem;
  text-align: center;
}

.modalButton {
  width: 100%;
  margin: 1rem 0;
  padding: 0.5rem 0;
  background-color: #2c3e50;
  color: #ecf0f1;
  border: none;
  border-radius: 0.25rem;
  font-size: 1.25rem;
  text-align: center;
}
.modalButton:hover {
  opacity: 0.75;
}
.modalButton:active {
  background-color: #34495e;
}
</style>
