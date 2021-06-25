<template>
  <aside class="modal-backdrop">
    <div class="modal">
      <h1>Konverze úspěšně uložena</h1>
      <p>Částka konverze: {{ this.savedAmount }}</p>
      <button @click="close">Close</button>
    </div>
  </aside>
</template>
<style scoped>
.modal-backdrop {
  position: absolute;
  border: 1px solid orange;
  top: 10%;
  left: 25%;
  width: 50%;
  height: 50%;
  display: flex;
  align-items: flex-start;
  justify-content: center;
  background-color: rgba(0, 0, 0, 0.9);
}

h1 {
  font-size: 2rem;
}

.modal {
  margin-top: 20%;
  width: 20rem;
  height: 20rem;
  color: saddlebrown;
}
</style>
<script>
export default {
  name: 'Modal',
  props: ['savedAmount'],
  methods: {
    close() {
      this.$emit('close');
      console.log(this.savedAmount);
    },
  },
};
</script>
