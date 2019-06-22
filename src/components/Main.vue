<template>
  <v-container>
    <v-layout>
      <v-flex mb-4>
        <v-flex xs12 sm9 md6>
          <v-textarea
            box
            name="participants"
            v-model="participants"
            label="Enter participant names here (1 per line)"
          ></v-textarea>
          <v-btn depressed small color="primary" @click="generatePairings">Generate Pairings</v-btn>
        </v-flex>

        <h1 class="display-2 font-weight-bold mb-3">
          Welcome to Vuetify
        </h1>
        <p class="subheading font-weight-regular">
          For help and collaboration with other Vuetify developers,
          <br>please join our online
          <a href="https://community.vuetifyjs.com" target="_blank">Discord Community</a>
        </p>
      </v-flex>

    </v-layout>
  </v-container>
</template>

<script>
  export default {
    data: () => ({
      participants: '',

    }),
    methods: {
      generatePairings() {
        console.log(this.participants)
      }
    }
  }
</script>

<style>

</style>
