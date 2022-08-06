<template>
  <v-app>
    <v-app-bar app color="primary" dark>
      <span style="font-size: 24px">My Contacts App</span>
    </v-app-bar>

    <v-main>
      <LandingPage />
    </v-main>
  </v-app>
</template>

<script lang="ts">
import Vue from "vue";
import LandingPage from "./components/LandingPage.vue";

export default Vue.extend({
  name: "App",

  components: {
    LandingPage,
  },
});
</script>
