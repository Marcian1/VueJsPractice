<template>
  <ul>
    <transition-group name="users">
      <li v-for="user in users" :key="user" @click="removeUser(user)">
        {{ user }}
      </li>
    </transition-group>
  </ul>
  <input type="text" ref="inputText" />
  <button @click="addUser()">Add User</button>
</template>

<script>
export default {
  data() {
    return {
      users: ['User1', 'User2', 'User3', 'User4']
    };
  },
  methods: {
    addUser() {
      const enteredValue = this.$refs.inputText.value;
      this.users.push(enteredValue);
    },
    removeUser(user) {
      console.log(user);
      this.users = this.users.filter(usr => usr !== user);
    }
  }
};
</script>

<style scoped>
ul {
  list-style: none; /* Removes the bullet points of the list */
  margin: 0;
  padding: 0;
}
li {
  border: 1px solid #ccc;
  padding: 1rem;
  text-align: center;
}

.users-enter-from {
  opacity: 0;
  transform: translateX(-30px);
}
.users-enter-active {
  transition: all 1s ease-out
}

.users-enter-to,
.users-leave-from {
  opacity: 1;
  transform: translateX(0);
}


.users-leave-to {
  opacity: 0;
  transform: translateX(30px);
}
.users-leave-active {
  transition: all 1s ease-in
}

</style>
