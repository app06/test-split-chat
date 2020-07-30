<template>
  <b-list-group>
    <b-list-group-item
      :class="[
        selectedUserId === user.id ? 'active' : '',
        'd-flex align-items-center app-contact list-group-item-action'
      ]"
      v-for="user in users"
      :key="user.id"
      @click="selectUser(user.id)"
    >
      <b-avatar
        class="mr-3 app-avatar"
        variant="info"
        :src="user.avatar"
      ></b-avatar>
      <span class="mr-auto">{{ user.nickname }}</span>
    </b-list-group-item>
  </b-list-group>
</template>

<script>
export default {
  name: 'Contacts',
  props: {
    users: {
      required: true,
      type: Array
    },
    selectedUserId: { required: true }
  },
  methods: {
    selectUser(id) {
      this.$emit('select-user', id);
    }
  }
};
</script>

<style>
.app-avatar {
  flex-shrink: 0;
  /* display: inline-flex;
  align-items: center;
  justify-content: center; */
}

.app-avatar img {
  max-width: 100%;
  border-radius: 50%;
}

.app-contact {
  cursor: pointer;
}

.app-contact span {
  user-select: none;
}
</style>
