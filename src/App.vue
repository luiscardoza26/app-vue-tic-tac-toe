<template>
  <div id="nav">
    <router-link class="btn btn-info" to="/">Nueva Partida</router-link> |
    <router-link class="btn btn-warning" to="/unirsep">Unirse A Partida</router-link>
  </div>
  <router-view/>
</template>

<style lang="scss">
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
}

#nav {
  padding: 30px;

  a {
    font-weight: bold;
    color: #ffff;

    &.router-link-exact-active {
      color: #ffff;
    }
  }
}
</style>
