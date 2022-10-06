<template>
  <li>
    <h2>{{ name }} {{ friendIsFavorite === '1' ? '(Favorite)' : '' }}</h2>

    <button @click="toggleFavorite">Toggle Favorite</button>

    <button @click="toggleDetails">
      {{ detailsAreVisible ? 'Hide' : 'Show' }} Details
    </button>

    <ul v-if="detailsAreVisible">
      <li><strong>Phone:</strong>{{ phoneNumber }}</li>
      <li><strong>Email:</strong>{{ emailAddress }}</li>
    </ul>
  </li>
</template>

<script>
  export default {
    // In its simplest form the props property takes an array
    // props are passed as strings and converted from kabob case to camelCase
    // props: ['name', 'phoneNumber', 'emailAddress'],

    props: {
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
        type: String,
        required: false,
        // default can also be a function with a more complex code snippet to derive the default value (default: function() {})
        default: '0',
        // A validator can also be added, it gets the value provided for the prop, it holds a function that should return true or false after running validation logic
        validator: function (value) {0
          return value === '1' || value === '0';
        },
      },
    },
    data() {
      return {
        detailsAreVisible: false,
        // Because unidirectional data flow is important in Vue as in react (cannot change the value of the passed in prop)
        // Also possible to inform the parent to update the properties/values of the passed in props => passing it back like in React
        friendIsFavorite: this.isFavorite,
      };
    },
    methods: {
      toggleDetails() {
        this.detailsAreVisible = !this.detailsAreVisible;
      },
      toggleFavorite() {
        if (this.friendIsFavorite === '1') {
          this.friendIsFavorite = '0';
        } else {
          this.friendIsFavorite = '1';
        }
      },
    },
  };
</script>
