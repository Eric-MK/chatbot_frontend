<template>
    <div>
      <input type="text" v-model="query" placeholder="Enter your medical question" @keyup.enter="sendQuery">
      <button @click="sendQuery">Ask</button>
      <div v-if="response">
        <p>{{ response }}</p>
      </div>
    </div>
</template>
  
<script>
import axios from 'axios';

export default {
data() {
    return {
    query: '',
    response: ''
    };
},
methods: {
    async sendQuery() {
    try {
        const { data } = await axios.post('http://localhost:5000/query', { query: this.query });
        this.response = data.response;
    } catch (error) {
        console.error('Error:', error);
    }
    }
}
};
</script>

<style scoped>
/* Add your component styles here */
</style>
  