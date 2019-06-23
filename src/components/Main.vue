<template>
  <v-container>
    <v-layout>
      <v-flex xs12 sm9 md6>
        <v-textarea
          box
          name="participantsTextArea"
          v-model="participantsTextArea"
          label="Enter participant names here (1 per line)"
        ></v-textarea>
        <v-btn depressed small color="primary" @click="generatePairings">Generate Pairings</v-btn>
      </v-flex>
      <v-flex xs3 sm3 md3>


        <p>PARTICIPANTS</p>
        {{ participants }}
        <br><br>
        <p>PAIRINGS</p>
        {{ pairings }}
        <br><br>
        <p>ROUNDS</p>
        {{ rounds }}
      </v-flex>
    </v-layout>
    <v-layout>
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
      participantsTextArea: 'dmitry\neric\nmelodie',
      pairings: [],
      rounds: []
    }),
    methods: {
      generatePairings() {
        this.rounds = this.convertParticipantsToRounds(this.participants)
        this.pairings = this.convertRoundsForDataTable(this.rounds)
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
      },
      headers() {
        let out = [
          {
            text: 'Participant',
            align: 'left',
            value: 'name'
          }
        ]
        for (let i = 1; i < this.participants.length + 1; i++) {
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
