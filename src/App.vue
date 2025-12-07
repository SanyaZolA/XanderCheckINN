<template>
  <img alt="logo" src="./assets/iconslogo.png">
  <MainText title="Проверка ИНН на самозанятого"/>
  <a href="https://github.com/xandercheckinn" target="_blank" rel="noopener noreferrer">GitHub</a>
  <SearchINN />
  <p class="pre">v 1.3.1</p>
</template>

<script>
import MainText from './components/MainText.vue'
import SearchINN from './components/SearchINN.vue';

export default {
  name: 'App',
  components: {
    MainText,
    SearchINN
  }
}

</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
  display: flex;
  flex-direction: column;
  align-items: center;
}
.pre {
  position: absolute;
  text-align: end;
  width: 100%;
  bottom: 0px;
  right: 20px;
}
</style>
