<template>
  <v-app>
    <v-app-bar
      dense
      dark
    >
      <v-toolbar-title>Countries Infos</v-toolbar-title>

    </v-app-bar>

    <v-main>

      <v-container fluid>

        <router-view/>
      </v-container>
    </v-main>

    <v-footer app>
      <!-- -->
    </v-footer>
  </v-app>
</template>

<script>

export default {
  name: 'App',

  components: {
  },

}
</script>
