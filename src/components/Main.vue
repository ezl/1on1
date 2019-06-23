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
      <v-flex xs12 sm9 md6>
        <v-data-table
          :headers="headers"
          :items="pairings"
        >
          <template v-slot:items="props">
            <td>{{ props.item.name }}</td>
            <td class="text-xs-right">fooo</td>
          </template>
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
        console.log('pairings', this.pairings)
        console.log('headers', this.headers)
      },
      convertParticipantsToPairings(participantsArray) {
        // for now just return something that's approximately the right shape
        // i'll flesh out the real algorithm later
        let pairings = []
        participantsArray.forEach(participant => {
          let newRow = {
            name: participant
          }
          for (let i = 1; i < this.headers.length; i++) {
            newRow[i] = i
          }
          pairings.push(newRow)
        })

        return pairings
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
