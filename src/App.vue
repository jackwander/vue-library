<template>
  <div id="app" class="container">
    <img src="./assets/logo.png" class="logo">
    <BookList/>
  </div>
</template>

<script>
import HelloWorld from './components/HelloWorld'
import BookList from './components/BookList'

export default {
  name: 'App',
  components: {
    HelloWorld,
    BookList,
  }
}
</script>

<style>
* {
  box-sizing:border-box;
}

.container {
  max-width: 600px;
  margin: 0 auto;
}

#app {
  font-family: 'Avenir', Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
.logo {
  display:block;
  margin: 24px auto;
  height:  75px;
}
</style>
