<template>
  <b-row>
    <b-col md="6" xl="4">
      <Contacts
        :users="users"
        :selected-user-id="selectedUserId"
        @select-user="selectUser"
      />
    </b-col>

    <b-col md="6" xl="8" v-if="selectedUserId">
      <Dialog
        :messages="messages"
        :chat-id="chatId"
        :selected-user-id="selectedUserId"
        @add-message="addMessage"
      />
    </b-col>
    <b-col md="6" xl="8" v-else>
      <b-card>
        <h5>Выберите контакт</h5>
      </b-card>
    </b-col>
  </b-row>
</template>

<script>
import Contacts from '@/components/chat/Contacts.vue';
import Dialog from '@/components/chat/Dialog.vue';

export default {
  name: 'Chat',
  props: {
    chatId: {
      required: true,
      type: Number
    },
    users: {
      required: true,
      type: Array
    },
    messages: {
      required: true,
      type: Array
    }
  },
  data: () => ({
    selectedUserId: null
  }),
  methods: {
    selectUser(id) {
      this.selectedUserId = id;
    },
    addMessage(message) {
      this.$emit('add-message', message);
    }
  },
  components: {
    Contacts,
    Dialog
  }
};
</script>

<style></style>
