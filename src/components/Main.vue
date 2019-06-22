<template>
  <v-container>
    <v-layout>
      <v-flex xs12 sm9 md6>
        <v-textarea
          box
          name="participants"
          v-model="participants"
          label="Enter participant names here (1 per line)"
        ></v-textarea>
        <v-btn depressed small color="primary" @click="generatePairings">Generate Pairings</v-btn>
      </v-flex>
    </v-layout>
  </v-container>
</template>

<script>
  export default {
    data: () => ({
      participants: '',
      pairings: []
    }),
    methods: {
      generatePairings() {
        console.log(this.participants)
        console.log(typeof(this.participants))
        let participantsArray = this.participants
          .split("\n")
          .map(function(item) {
            return item.trim()
          })
          .filter(i => i !== "")
        this.pairings = this.convertParticipantsToPairings(participantsArray)
        // OK so this will overwrite this.pairings.
        // This.pairings will originally be empty (so there will be an empty table, which we will hide when empty)
        // once the button is pushed, we grab the data, turn it into pairings, then overwrite the pairings in `data`,
        // which will cause it to automatically re-render // sound ok?

      },
      convertParticipantsToPairings(participantsArray) {
        // for now just return something that's approximately the right shape
        // i'll flesh out the real algorithm later
        let periods = []
        for (let i = 0; i < participantsArray.length; i++) {
          periods.push(participantsArray)
        }
        console.log(periods)
        return periods
      }
    }
  }
</script>

<style>

</style>
