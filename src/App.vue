<template>
  <img alt="logo" src="./assets/iconslogo.png">
  <StartPage msg="Проверка ИНН на самозанятого"/>
  <PostUser />
</template>

<script>
import StartPage from './components/StartPage.vue'
import PostUser from './components/PostUser.vue';

export default {
  name: 'App',
  components: {
    StartPage,
    PostUser
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
</style>
