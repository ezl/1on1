<template>
  <v-app>
    <v-toolbar app>
      <v-toolbar-title class="headline text-uppercase">
        <span>Generate 1 on 1 pairings</span>
      </v-toolbar-title>
      <v-spacer></v-spacer>
      <v-btn
        flat
        href="https://github.com/ezl/"
        target="_blank"
      >
        <span class="mr-2">Github</span>
      </v-btn>
    </v-toolbar>

    <v-content>
      <Main/>
    </v-content>
  </v-app>
</template>

<script>
import Main from './components/Main'

export default {
  name: 'App',
  components: {
    Main
  },
  data () {
    return {
      //
    }
  }
}
</script>
