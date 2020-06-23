<template>
  <div>
    <v-layout justify-center>
      <v-card
        class="d-flex justify-center mb-6"
        flat
        tile
      >
        <h1>(404) Page Not Found</h1>
      </v-card>
    </v-layout>
    <v-layout justify-center>
      <v-card-actions>
        <v-btn primary>
          <router-link to='/'><span>Go Back</span></router-link>
        </v-btn>

      </v-card-actions>
    </v-layout>
  </div>
</template>

<script>
export default {};
</script>