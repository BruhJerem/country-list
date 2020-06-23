<template>
  <v-app>
    <v-app-bar
      dense
      dark
    >
      <v-toolbar-title>Countries Infos</v-toolbar-title>

    </v-app-bar>

    <!-- Sizes your content based upon application components -->
    <v-main>

      <!-- Provides the application the proper gutter -->
      <v-container fluid>

        <!-- If using vue-router -->
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
