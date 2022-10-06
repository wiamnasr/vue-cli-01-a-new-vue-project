<template>
  <li>
    <h2>{{ name }} {{ isFavorite ? '(Favorite)' : '' }}</h2>

    <button @click="toggleFavorite">Toggle Favorite</button>

    <button @click="toggleDetails">
      {{ detailsAreVisible ? 'Hide' : 'Show' }} Details
    </button>

    <ul v-if="detailsAreVisible">
      <li><strong>Phone:</strong>{{ phoneNumber }}</li>
      <li><strong>Email:</strong>{{ emailAddress }}</li>
    </ul>
    <button @click="deleteFriendInfo">Delete</button>
  </li>
</template>

<script>
  export default {
    // In its simplest form the props property takes an array
    // props are passed as strings and converted from kabob case to camelCase
    // props: ['name', 'phoneNumber', 'emailAddress'],

    props: {
      id: {
        type: String,
        required: true,
      },
      // We can simply provide type
      //   name: String,
      name: {
        // Better way of controlling what's being passed in by utilizing Vue's provided properties on props:
        type: String,
        required: true,
      },
      phoneNumber: { type: String, required: true },
      emailAddress: { type: String, required: true },

      //   Here is favorite is not required, so we provide a default key with a default value to be used if the prop is missing
      isFavorite: {
        type: Boolean,
        required: false,
        // default can also be a function with a more complex code snippet to derive the default value (default: function() {})
        default: false,
        // A validator can also be added, it gets the value provided for the prop, it holds a function that should return true or false after running validation logic
        validator: function (value) {
          return value === false || value === true;
        },
      },
    },
    // Adding a new property + $emits => Counterpart of props
    // Here we define which custom events our component will emit => Helps Better document the component and make sure other developers know to which events they can listen
    // Basic form of using it:
    // emits: ['toggle-favorite'],

    // Better configuration just like the props
    emits: {
      // We should add the function that will receive the data to be emitted as parameters
      // Making it clear that toggle-favorite is an event that should be handled by a function that expects an id to be emitted
      'toggle-favorite': function (id) {
        // We can add validation here to make sure when the event is emitted, this.data which should be part of the event, is not forgotten
        if (id) {
          console.log(id);
          return true;
        } else {
          console.log('no valid id');
          return false;
        }
      },
      'delete-friend-info': function (id) {
        if (id) {
          return true;
        } else {
          return false;
        }
      },
    },

    data() {
      return {
        detailsAreVisible: false,
        // Because unidirectional data flow is important in Vue as in react (cannot change the value of the passed in prop)
        // Also possible to inform the parent to update the properties/values of the passed in props => passing it back like in React
        // friendIsFavorite: this.isFavorite,
      };
    },
    methods: {
      toggleDetails() {
        this.detailsAreVisible = !this.detailsAreVisible;
      },
      toggleFavorite() {
        // this.friendIsFavorite = !this.friendIsFavorite;
        // using the built in method instead provided by Vue
        // $emit() is a built in method that can be called inside of a Vue component on the "this" keyword
        // Allows us to emit custom events, to which we can then listen to inside the parent component (communication child > parent)
        // IMPORTANT: use kebob casing no matter where you use it (in comparison to props where kebob is used to pass the value and camel casing to receive it)

        // Emitting the toggle-favorite event
        // Back in App.Vue we can listen to it in the component with v-on or the @ shorthand
        // This event should carry some data to let the App.Vue (parent) component know which of the 2 contacts changed its favorite status
        // This is accomplished by passing a second argument, here we expect to get the friend id as an extra prop
        this.$emit('toggle-favorite', this.id);
      },
      deleteFriendInfo() {
        this.$emit('delete-friend-info', this.id);
      },
    },
  };
</script>
