<template>
  <div id="app">
    <div class="app-shell">
      <header class="app-header">
        <div class="brand">
          <img alt="logo" src="./assets/iconslogo.png" class="logo" />
          <div class="brand-text">
            <h1>Проверка статуса самозанятого</h1>
          </div>
        </div>
      </header>

      <main class="card">
        <ApiFns />
        <span class="version">v 1.5</span>
      </main>
    </div>
  </div>
</template>

<script>
import ApiFns from "./components/ApiFNS.vue";

export default {
  name: "App",
  components: {
    ApiFns,
  },
};
</script>

<style>
* {
  box-sizing: border-box;
}

body {
  margin-top: 150px;
}

#app {
  max-height: 100vh;
  font-family: Inter, Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  display: flex;
  align-items: center;
  justify-content: center;
}

.app-shell {
  width: 100%;
  max-width: 700px;
}

.app-header {
  display: flex;
  justify-content: space-between;
  align-items: center;
  margin-bottom: 20px;
  gap: 5px;
}

.brand {
  display: flex;
  align-items: center;
  margin-left: 25px;
  gap: 10px;
}

.logo {
  width: 52px;
  height: 52px;
  object-fit: contain;
}

.brand-text h1 {
  margin: 0;
  font-size: 20px;
  font-weight: 700;
}

.brand-text p {
  margin: 4px 0 0;
  font-size: 14px;
  color: #6b7280;
}

.version {
  font-size: 13px;
  color: #94a3b8;
  padding: 100px 10px;
  border-radius: 999px;
}

.card {
  background: #fff;
  border-radius: 24px;
  margin: 20px;
  min-width: 500px;
  padding: 32px 24px 15px 24px;
  box-shadow: 0 20px 50px rgba(15, 23, 42, 0.12);
  border: 1px solid rgba(226, 232, 240, 0.9);
}

@media (max-width: 640px) {
  .app-header {
    flex-direction: column;
    align-items: flex-start;
  }

  .brand-text h1 {
    font-size: 19px;
  }

  .logo {
    width: 30px;
    height: 30px;
  }
  .card {
    padding: 24px 18px;
    border-radius: 20px;
    min-width: 350px;
  }
}
</style>
