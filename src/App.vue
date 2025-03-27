<template>
  <img alt="logo" src="./assets/iconslogo.png">
  <StartPage title="Проверка ИНН на самозанятого"/>
  <PostUser />
  <p class="pre">v 1.2</p>
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
.pre {
  position: absolute;
  text-align: end;
  width: 100%;
  bottom: 0px;
  right: 20px;
}
</style>
