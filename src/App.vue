<script setup>
import MineSweeper from './components/MineSweeper.vue'
</script>

<template>
  <main>
    <MineSweeper msg="MineSweeper"/>
  </main>
</template>

<style scoped>
header {
  line-height: 1.5;
}

.logo {
  display: block;
  margin: 0 auto 2rem;
}

@media (min-width: 1024px) {
  header {
    display: flex;
    place-items: center;
    padding-right: calc(var(--section-gap) / 2);
  }

  .logo {
    margin: 0 2rem 0 0;
  }

  main{
    display: flex;
    align-items: center;
  }
}
</style>
