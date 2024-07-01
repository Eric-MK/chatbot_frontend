<template>
  <div id="app">
    <h1>Medical Chatbot</h1>
    <Chatbot />
  </div>
</template>

<script>
import Chatbot from './components/Chatbot_component.vue';

export default {
  name: 'App',
  components: {
    Chatbot
  }
};
</script>

<style>
/* Add your global styles here */
</style>
