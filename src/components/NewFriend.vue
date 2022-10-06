<template>
  <form @submit.prevent="addToFriendsList">
    <div>
      <label>Name</label>
      <input
        type="text"
        v-model="inputName"
      />
    </div>
    <div>
      <label>Phone</label>
      <input
        type="tel"
        v-model="inputPhone"
      />
    </div>
    <div>
      <label>Email</label>
      <input
        type="email"
        v-model="inputEmail"
      />
    </div>
    <button>Add Contact</button>
  </form>
</template>

<script>
  export default {
    emits: {
      'add-to-friends-list': function (friendInfo) {
        if (
          friendInfo.name &&
          friendInfo.id &&
          friendInfo.phone &&
          friendInfo.email
        ) {
          console.log(friendInfo);
          return true;
        } else {
          console.log('invalid emit');
          console.log(friendInfo);
          return false;
        }
      },
    },
    data() {
      return {
        inputName: '',
        inputPhone: '',
        inputEmail: '',
      };
    },
    methods: {
      addToFriendsList() {
        console.log('adding to friends list: ');
        console.log(`${this.inputName} - ${this.inputEmail}`);

        this.$emit('add-to-friends-list', {
          id: `${this.inputName} ${this.inputEmail}`,
          name: this.inputName,
          phone: this.inputPhone,
          email: this.inputEmail,
        });

        this.inputName = '';
        this.inputPhone = '';
        this.inputEmail = '';
      },
    },
  };
</script>
