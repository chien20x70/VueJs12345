<template>
  <div id="app">
    <NavBar/>
    <Header/>
    <Content/>
  </div>
</template>

<script>
import NavBar from './components/layout/NavBar.vue'
import Header from './components/layout/Header.vue'
import Content from './components/layout/Content.vue'

export default {
  name: 'App',
  components: {
    NavBar,
    Header,
    Content,
  }
}
</script>

<style>
  @import './components/styles/main.css';
  @import './components/styles/button.css';
  @import './components/styles/input.css';
  @import './components/styles/dropdown.css';
  @import './components/styles/table.css';

</style>
