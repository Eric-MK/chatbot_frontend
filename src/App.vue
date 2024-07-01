<template>
  <div id="app">
    <h1 class="sticky-header">Medical Chatbot</h1>
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

.sticky-header {
  background-color: #eaedf1;
  color: #0d0c0c;
  
  padding: 10px;
  text-align: center;
  position: sticky;
  top: 0;
  z-index: 1000; /* Ensure it's above other content */
}
</style>
