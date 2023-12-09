<template>
  <CustomForm />
  <div ref="result" class="result">

  </div>
</template>

<script>
import CustomForm from './components/CustomForm.vue';

export default {
  name: 'App',
  components: {
    CustomForm
  },
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

body {
  background: #e0e0e0;
}
</style>
