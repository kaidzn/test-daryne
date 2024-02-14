<template>
  <FirstPart/>
  <SecondPart/>
</template>

<script>
import FirstPart from "./components/FirstPart.vue"
import SecondPart from "./components/SecondPart.vue";

export default {
  name: 'App',
  components: {
    FirstPart,
    SecondPart
}
}
</script>

<style>
@import url('https://fonts.googleapis.com/css2?family=Inter:wght@100..900&display=swap');

*{
  margin: 0;
  padding: 0;
  box-sizing: border-box;
}
body{
  background-color: #fafafa;
  font-family: 'Inter', sans-serif;
  color: #495057  ;
}
</style>
