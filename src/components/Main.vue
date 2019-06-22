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
      <br><!-- stupid i'll figure out later -->
      <v-flex xs12 sm9 md6>
        <v-data-table
          :items="pairings"
        >
        </v-data-table>
      </v-flex>
    </v-layout>
  </v-container>
</template>

<script>
  export default {
    data: () => ({
      participants: 'dmitry\neric\nmelodie',
      pairings: []
    }),
    methods: {
      generatePairings() {
        console.log('participantsArray', this.participantsArray, this.participantsArray.length)
        this.pairings = this.convertParticipantsToPairings(this.participantsArray)
        console.log('headers', this.headers)
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
    },
    computed: {
      participantsArray() {
        return this.participants
          .split("\n")
          .map(function(item) {
            return item.trim()
          })
          .filter(i => i !== "")
      },
      headers() {
        let out = [
          {
            text: 'Participant',
            align: 'left',
            value: 'name'
          }
        ]

        for (let i = 1; i < this.participantsArray.length + 1; i++) {
          out.push({
            text: i,
            value: i
          })
        }

        return out
      }
    }
  }
</script>

<style>

</style>
