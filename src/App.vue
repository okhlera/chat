<template>
  <div id="app">
    <Container>
      <ChatWindow @send-message="sendMessages">
        <ChatMessage v-for="message in messages" :key="message.id" :username="message.author" :datetime="message.datetime">{{message.text}} </ChatMessage>
      </ChatWindow>
    </Container>
  </div>
</template>

<script>
import axios from 'axios'
import ChatMessage from './components/ChatMessage.vue'
import ChatWindow from './components/ChatWindow.vue'
import Container from './components/Container.vue'

export default {
  name: 'app',
  components: {
    ChatWindow,ChatMessage,Container
  },
  data(){
    return {messages:[]}
},
  methods:{
    getMessages(){
      axios.get('http://188.225.47.187/api/chat/getmessages.php').then((response)=>{
        this.messages=response.data.sort((a,b) => a.id - b.id)
    })
    },
    sendMessages(obj){
      axios.post('http://188.225.47.187/api/chat/sendmessage.php',{author: obj.nickname,
        text: obj.message}).then(()=>{
        this.getMessages()
      })
    }

  },
    mounted(){
      setInterval(()=>{
        this.getMessages()},3000
      )

  },
  }  
  

</script>

<style>
#app {
  font-family: 'Avenir', Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
body {
  margin: 0;
  background-color: #f9f9fa;
}
</style>