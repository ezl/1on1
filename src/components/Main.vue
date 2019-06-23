<template>
  <v-container>
    <v-layout>
      <v-flex sm12 md4>
        <label for="participantsTextArea">Enter participant names here (1 per line)</label>
        <v-textarea
          box
          name="participantsTextArea"
          v-model="participantsTextArea"
          label="Participant names"
        ></v-textarea>
        <v-btn depressed small color="primary" @click="generatePairings">Generate Pairings</v-btn>
        <v-btn depressed small @click="clear">Clear</v-btn>
      </v-flex>
      <v-flex sm12 md8 v-if="pairings.length">
        <v-data-table
          :headers="headers"
          :items="pairings"
          hide-actions
        >
          <template v-slot:items="props" >
            <td v-for="header in headers" class="text-xs">{{ props.item[header.value] }}</td>
          </template>
        </v-data-table>
      </v-flex>
    </v-layout>
  </v-container>
</template>

<script>
  export default {
    data: () => ({
      participantsTextArea: 'dmitry\neric\nmelodie',
      headers: [],
      pairings: [],
      rounds: []
    }),
    methods: {
      clear() {
        this.participantsTextArea = []
        this.pairings = []
        this.rounds = []
      },

      generatePairings() {
        this.headers = this.generateHeaders()
        this.rounds = this.convertParticipantsToRounds(this.participants)
        this.pairings = this.convertRoundsForDataTable(this.rounds)
      },

      generateHeaders() {
        let out = [
          {
            text: 'Participant',
            align: 'left',
            value: 'name'
          }
        ]

        let roundsRequired = (this.participants.length % 2 === 1) ? this.participants.length : this.participants.length - 1

        for (let i = 1; i < roundsRequired; i++) {
          out.push({
            sortable: false,
            text: i,
            value: i
          })
        }
        return out
      },

      convertParticipantsToRounds(participants) {
        /* Returns an Array of pairings for each period like:
           [
             [  [1,2], [3,4], [5, 6]  ],   // period 1 pairings
             [  [1,3], [5,4], [2, 6]  ],   // period 2 pairings
             ...
           ]

           This isn't a great format for humans to look stuff up though, so we'll transform it later

           */
        let DUMMY = -1
        var rounds = []

        let n = participants.length

        let participantsIsOdd = n % 2 === 1
        if (participantsIsOdd) {
          // so we can match algorithm for even numbers
          participants.push(DUMMY)
          n += 1
        }
        for (var j = 0; j < n - 1; j += 1) {
          let round = []
          for (var i = 0; i < n / 2; i += 1) {
            if (participants[i] !== DUMMY && participants[n - 1 - i] !== DUMMY) {
              // hide byes
              round.push([participants[i], participants[n - 1 - i]]) // insert pair as a match
            }
          }
          rounds.push(round)
          participants.splice(1, 0, participants.pop()) // permutate for next round
        }
        if (participantsIsOdd === true) {
          participants.pop(participants.indexOf(DUMMY))
        }
        return rounds
      },

      convertRoundsForDataTable(rounds) {
        /* Takes the output of convertParticipantsToPairings
           and makes it easier to look up by participant

           Converts:
           [
             [a,b],
             [a,c],
             [b,c]
           ]

           To:
           [
             {name:a, 1:b, 2:c},
             {name:b, 1:a, 3:c},
             {name:c, 2:a, 3:b}
           ]
           */

        let output = []
        this.participants.forEach(participant => {
          output.push({name: participant})
        })

        // oof this whole this is pretty fragile. if anything else
        // changes, this needs to be rewritten
        // for efficiency, this relies on the fact that
        // this.participants stays in the same order, then we push
        // them in that order onto output, then we use that same
        // ordering to create the output dict for each period
        for(let i = 0; i < rounds.length; i++) {
          let round = rounds[i]
          console.log(rounds)
          console.log(round)
          let roundNumber = i + 1 // 0 indexed, the 1st round is the 0th element
          round.forEach(pairing => {
            let a = pairing[0] // person A in a pairing in this round
            let b = pairing[1] // person B in a pairing in this round
            console.log("a =>", a)
            console.log("b =>", b)
            console.log(this.participants)

            output[this.participants.indexOf(a)][roundNumber] = b
            output[this.participants.indexOf(b)][roundNumber] = a
          }, this)
        }

        return output
      }
    },
    computed: {
      participants() {
        return this.participantsTextArea
          .split("\n")
          .map(function(item) {
            return item.trim()
          })
          .filter(i => i !== "")
      }
    }
  }
</script>

<style>

</style>
