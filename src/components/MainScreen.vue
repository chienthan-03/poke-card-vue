<template lang="">
  <div class="screen">
    <img
      alt="Vue logo"
      class="img__main"
      src="../assets/images/icon_back.png"
    />
    <h1>POKE MEMORIES</h1>
    <h3>Select mode to start game</h3>
    <div class="modes">
      <button @click="onStart(16)">
        <span>4x4</span>
        <span>Easy</span>
      </button>
      <button @click="onStart(36)">
        <span>6x6</span>
        <span>Normal</span>
      </button>
      <button @click="onStart(64)">
        <span>8x8</span>
        <span>Hard</span>
      </button>
      <button @click="onStart(100)">
        <span>10x10</span>
        <span>Super hard</span>
      </button>
    </div>
  </div>
</template>
<script>
export default {
  methods: {
    onStart(totalOfBlocks) {
      this.$emit("onStart", { totalOfBlocks });
    },
  },
};
</script>
<style lang="css" scoped>
.img__main {
  width: 100px;
  height: 100px;
}
.screen {
  width: 100%;
  height: 100vh;
  position: absolute;
  top: 0;
  left: 0;
  z-index: 2;
  display: flex;
  align-items: center;
  justify-content: center;
  flex-direction: column;
  background-color: #212121;
  color: #f3f3f3;
}
.screen h1 {
  font-size: 4rem;
  text-transform: uppercase;
}
.screen p {
  font-size: 2rem;
}
.modes {
  display: flex;
  flex-wrap: wrap;
  justify-content: center;
  align-items: center;
  margin-top: 2rem;
}
.modes button {
  font: var(--font);
  width: 150px;
  height: 150px;
  background: transparent;
  box-shadow: none;
  border: 1px solid #f3f3f3;
  color: #f3f3f3;
  display: flex;
  flex-direction: column;
  border-radius: 1rem;
  margin: 1rem;
  align-items: center;
  justify-content: center;
  cursor: pointer;
  transition: background 0.3s ease-in-out;
}
.modes button:hover {
  background-color: #f3f3f3;
  color: #212121;
}
.modes button span:first-child {
  font-size: 2rem;
}
.modes button span:last-child {
  display: block;
  font-size: 1.25rem;
  margin-top: 0.5rem;
}
</style>
