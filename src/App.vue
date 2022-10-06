<template>
  <h2>Testing outputting lists</h2>
  <new-friend @add-to-friends-list="addToFriendsList"></new-friend>
  <ul>
    <friend-contact
      v-for="friend in friends"
      :key="friend.id"
      :id="friend.id"
      :name="friend.name"
      :phone-number="friend.phone"
      :email-address="friend.email"
      :is-favorite="friend.favorite"
      @toggle-favorite="toggleFavoriteStatus"
    />
  </ul>
</template>

<script>
  // Inside the script tag, we provide the configuration for out vue app that we want to mount
  export default {
    data() {
      return {
        friends: [
          //   {
          //     id: 'wiam',
          //     name: 'W N',
          //     phone: '01 234 546 542',
          //     email: 'xyz@zyx.com',
          //     favorite: false,
          //   },
          //   {
          //     id: 'test',
          //     name: 'test',
          //     phone: '01 234 111 542',
          //     email: 'test@zyx.com',
          //     // Not providing a favorite value here to demonstrate default prop value setting in components
          //   },
        ],
      };
    },
    methods: {
      toggleFavoriteStatus(friendId) {
        const identifiedFriend = this.friends.find(
          (friend) => friend.id === friendId
        );
        identifiedFriend.favorite = !identifiedFriend.favorite;
      },

      addToFriendsList(friendInfo) {
        console.log(friendInfo);
        this.friends.push(friendInfo);
      },
    },
  };
</script>

<style>
  @import url('https://fonts.googleapis.com/css2?family=Jost&display=swap');
</style>
