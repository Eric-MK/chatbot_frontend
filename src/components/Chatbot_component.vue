<template>
    <div class="chatbot-container">
      <div class="chatbot">
        <div class="chatbot-header">
          Medical Chatbot
        </div>
        <div class="chatbot-messages">
          <div v-for="(message, index) in messages" :key="index" class="message">
            <p v-if="message.type === 'user'" class="user-message">{{ message.text }}</p>
            <p v-else class="bot-message">{{ message.text }}</p>
          </div>
        </div>
        <div class="chatbot-input">
          <input type="text" v-model="query" placeholder="Enter your medical question" @keyup.enter="sendQuery" class="input-field">
          <button @click="sendQuery" class="ask-button">Ask</button>
          <div v-if="loading" class="loading-animation"></div>
        </div>
      </div>
    </div>
  </template>
  
  <script>
  import axios from 'axios';
  
  export default {
    data() {
      return {
        query: '',
        messages: [],
        loading: false // Add loading state
      };
    },
    methods: {
      async sendQuery() {
        if (!this.query.trim()) {
          return; // Prevent sending empty queries
        }
        this.loading = true; // Start loading animation
  
        // Add user message to chat history
        this.messages.push({ type: 'user', text: this.query });
  
        try {
          const { data } = await axios.post('http://localhost:5000/query', { query: this.query });
          // Add bot response to chat history
          this.messages.push({ type: 'bot', text: data.response });
          this.query = ''; // Clear input field after sending query
        } catch (error) {
          console.error('Error:', error);
          let errorMessage = 'Failed to get response from server. Please try again later.';
          if (error.response && error.response.data && error.response.data.message) {
            errorMessage = error.response.data.message;
          }
          // Add error message to chat history
          this.messages.push({ type: 'bot', text: errorMessage });
        } finally {
          this.loading = false; // Stop loading animation
        }
      }
    }
  };
  </script>
  
  <style scoped>
  /* Add scoped component styles */
  
  .loading-animation {
    width: 20px;
    height: 20px;
    border: 2px solid rgba(0, 123, 255, 0.2);
    border-radius: 50%;
    border-top-color: #007bff;
    animation: spin 1s linear infinite;
    margin-left: 10px;
  }
  
  @keyframes spin {
    0% {
      transform: rotate(0deg);
    }
    100% {
      transform: rotate(360deg);
    }
  }
  
  /* Add scoped component styles */
  .chatbot-container {
    display: flex;
    justify-content: center;
    align-items: center;
    height: 100vh;
    background-color: #f0f0f0;
  }
  
  .chatbot {
    max-width: 400px;
    width: 100%;
    background-color: #fff;
    border-radius: 8px;
    box-shadow: 0 0 10px rgba(0, 0, 0, 0.1);
    overflow: hidden;
  }
  
  .chatbot-header {
    background-color: #007bff;
    color: #fff;
    font-size: 1.2rem;
    padding: 10px;
    text-align: center;
  }
  
  .chatbot-messages {
    max-height: 300px;
    overflow-y: auto;
    padding: 10px;
  }
  
  .message {
    margin-bottom: 10px;
  }
  
  .user-message {
    background-color: #e2f3f5;
    padding: 8px;
    border-radius: 5px;
    max-width: 70%;
  }
  
  .bot-message {
    background-color: #d4edda;
    padding: 8px;
    border-radius: 5px;
    max-width: 70%;
  }
  
  .chatbot-input {
    display: flex;
    align-items: center;
    padding: 10px;
  }
  
  .input-field {
    flex: 1;
    padding: 8px;
    border: 1px solid #ccc;
    border-radius: 5px;
  }
  
  .ask-button {
    margin-left: 10px;
    padding: 8px 16px;
    border: none;
    background-color: #007bff;
    color: #fff;
    border-radius: 5px;
    cursor: pointer;
  }
  
  .ask-button:hover {
    background-color: #0056b3;
  }
  
  .ask-button:active {
    background-color: #0056b3;
    transform: translateY(1px);
  }
  </style>
  