<template>
  <div id="app">
    <WelcomeForm @add-user="addUser" v-if="!session" />
    <b-container fluid v-if="session" class="app-container">
      <b-row>
        <b-col xl="6" class="first-chat">
          <Chat
            :users="users"
            :messages="messages"
            :chatId="session.id"
            @add-message="addMessage"
          />
        </b-col>
        <b-col xl="6">
          <Chat
            :users="users"
            :messages="messages"
            :chatId="1"
            @add-message="addMessage"
          />
        </b-col>
      </b-row>
    </b-container>
  </div>
</template>

<script>
import WelcomeForm from '@/components/WelcomeForm.vue';
import Chat from '@/components/chat/Chat.vue';

export default {
  name: 'app',
  data: () => ({
    session: false,
    users: [
      {
        id: 1,
        nickname: 'Иван Иванов',
        avatar: '/avatars/1.png'
      }
    ],
    messages: []
  }),
  components: {
    WelcomeForm,
    Chat
  },
  methods: {
    addUser(user) {
      this.session = user;
      this.users.push(user);
    },
    addMessage(message) {
      this.messages.push(message);
    }
  }
};
</script>

<style>
.app-container {
  padding-top: 50px;
  max-width: 1920px;
  margin-left: auto;
  margin-right: auto;
}

@media (max-width: 1199px) {
  .first-chat {
    margin-bottom: 30px;
  }
}
</style>
