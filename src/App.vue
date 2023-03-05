<template>
  <TimeTrans />
  <PassWord />

</template>

<script>
import PassWord from './components/password.vue'
import TimeTrans from './components/time.vue'

export default {
  name: 'App',
  components: {
    PassWord,
    TimeTrans
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
}
</style>
