<template>
  <div class="screen">
    <h1>✨ Congratulations ✨</h1>
    <h3>{{ Math.round(timer / 1000) }} senconds</h3>
    <button @click="onStartAgain">Start again</button>
  </div>
</template>

<script>
export default {
  props: {
    timer: Number,
  },
  methods: {
    onStartAgain() {
      this.$emit("onStartAgain");
    },
  },
};
</script>

<style lang="css" scoped>
.screen {
  width: 100%;
  height: 100vh;
  position: absolute;
  top: 0;
  left: 0;
  z-index: 2;
  background-color: var(--dark);
  color: var(--light);
  display: flex;
  align-items: center;
  justify-content: center;
  flex-direction: column;
}
.screen h1 {
  font-size: 5rem;
}
.screen h3 {
  margin-top: 1.5rem;
  font-size: 3rem;
}

.screen button {
  font: var(--font);
  width: 200px;
  height: 100px;
  background: transparent;
  box-shadow: none;
  border: 1px solid var(--light);
  color: var(--light);
  display: flex;
  flex-direction: column;
  border-radius: 1rem;
  margin: 0 1rem;
  align-items: center;
  justify-content: center;
  cursor: pointer;
  transition: background 0.3s ease-in-out;
  font-size: 20px;
}
.screen button:hover {
  background-color: var(--light);
  color: var(--dark);
}
</style>
