<template>
  <b-card class="dialog">
    <div class="dialog-messages" ref="dialogMessages">
      <div
        v-for="message in messagesByUser"
        :key="message.id"
        :class="[message.from === chatId ? 'owner' : '', 'message']"
      >
        <div class="message-text">
          {{ message.text }}
        </div>
      </div>
    </div>
    <div class="dialog-form">
      <form @submit.prevent="sendMessage">
        <b-form-textarea
          v-model="text"
          v-model.trim="message"
          @keyup.enter.exact="sendMessage"
        ></b-form-textarea>
        <button type="submit">
          <b-icon-arrow-right-circle></b-icon-arrow-right-circle>
        </button>
      </form>
    </div>
  </b-card>
</template>

<script>
export default {
  name: 'Dialog',
  props: {
    chatId: {
      required: true,
      type: Number
    },
    messages: {
      required: true,
      type: Array
    },
    selectedUserId: { required: true }
  },
  data: () => ({
    message: ''
  }),
  watch: {
    selectedUserId(newVal, oldVal) {
      if (newVal !== oldVal) {
        this.scrollMessages();
        this.message = '';
      }
    },
    messagesByUser() {
      this.scrollMessages();
    }
  },
  computed: {
    messagesByUser() {
      if (this.selectedUserId === this.chatId) {
        return this.messages.filter(
          message => message.from === this.chatId && message.to === this.chatId
        );
      }
      return this.messages.filter(
        message =>
          (message.to === this.chatId &&
            message.from === this.selectedUserId) ||
          (message.from === this.chatId && message.to === this.selectedUserId)
      );
    }
  },
  methods: {
    scrollMessages() {
      this.$nextTick(function() {
        const el = this.$refs.dialogMessages;
        el.scrollTop = el.scrollHeight;
      });
    },
    sendMessage() {
      if (!this.message) return;

      const message = {
        id: new Date().getTime(),
        text: this.message,
        from: this.chatId,
        to: this.selectedUserId
      };

      this.message = '';
      this.$emit('add-message', message);
      this.scrollMessages();
    }
  }
};
</script>

<style>
.dialog {
  min-height: 400px;
  padding-bottom: 75px;
}

.dialog textarea.form-control {
  resize: none;
  height: 38px;
  padding-right: 30px;
  overflow: hidden;
  outline: none;
}

.dialog textarea.form-control:focus {
  border-color: #ced4da;
  box-shadow: none;
}

.dialog-form {
  position: absolute;
  bottom: 1.25rem;
  left: 1.25rem;
  right: 1.25rem;
}

.dialog-form button {
  position: absolute;
  border: none;
  background: transparent;
  padding: 0;
  font-size: 25px;
  color: #ced4da;
  right: 5px;
  top: -3px;
  outline: none;
  transition: color 0.3s ease-in-out;
}

.dialog-form button:hover {
  color: #000;
}

.dialog-messages {
  position: absolute;
  top: 1.25rem;
  left: 1.25rem;
  right: 1.25rem;
  bottom: 75px;
  overflow-y: auto;
  overflow-x: hidden;
}

.message {
  margin-bottom: 10px;
  text-align: left;
}

.message-text {
  display: inline-block;
  background-color: #d1ecf1;
  border-radius: 0.25rem;
  padding: 0.2rem 0.4rem;
  margin-right: 5%;
  max-width: 95%;
}

.message.owner .message-text {
  background-color: #f8f9fa;
  margin-right: 0;
  margin-left: 5%;
}

.message.owner {
  text-align: right;
}

@media (max-width: 1199px) {
  .dialog {
    min-height: 200px;
  }
}
</style>
