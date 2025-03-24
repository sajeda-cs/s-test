<!-- App.vue -->
<template>
  <div class="app-container">
    <AppSidebar />
    <div class="main-content">
      <AppNavbar />
      <div class="content">
        <ChatView />
      </div>
    </div>
  </div>
</template>

<script>
import AppSidebar from './components/AppSidebar.vue';
import AppNavbar from './components/AppNavbar.vue';
import ChatView from './components/ChatView.vue';

export default {
  components: {
    AppSidebar,
    AppNavbar,
    ChatView,
  },
};
</script>

<style>
* {
  margin: 0;
  padding: 0;
  box-sizing: border-box;
  font-family: 'Poppins', sans-serif;
}

body {
  background-color: #ffff;
}

.app-container {
  display: flex;
  width: 100%;
  min-height: auto;
}

.main-content {
  flex: 1;
  display: flex;
  flex-direction: column;
  margin-left: 180px; 
}

.content {
  flex: 1;
  padding: 20px;
  padding-top: 120px;
  background-color: #F4F7FB;
  height: calc(100vh - 120px); 
  overflow-y: auto; 
}


::-webkit-scrollbar {
  width: 5px;
  height: 5px;
}

::-webkit-scrollbar-track {
  background: #ffff;
}

::-webkit-scrollbar-thumb {
  background: #ffff; 
  border-radius: 10px;
}

::-webkit-scrollbar-thumb:hover {
  background: #bbb;
}

.chat-container {
  height: 100%;
  display: flex;
  flex-direction: column;
}

.chat-messages {
  flex: 1;
  overflow-y: auto;
  max-height: calc(100vh - 200px);
}

.chat-input {
  margin-top: auto;
  padding: 10px 0;
}
</style>