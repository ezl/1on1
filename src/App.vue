<template>
  <v-app>
    <v-toolbar
      flat
      app
      dense
      color=transparent
    >
      <v-toolbar-title class="headline">
        <span class="primary--text">Generate 1-on-1 pairings</span>
      </v-toolbar-title>
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
