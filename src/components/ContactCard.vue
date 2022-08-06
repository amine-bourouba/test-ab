<template>
  <div>
    <v-card v-if="contact">
      <v-img
        :src="contact.src"
        class="white--text align-end"
        gradient="to bottom, rgba(0,0,0,.1), rgba(0,0,0,.5)"
        height="250px"
      />

      <v-card-actions>
        <v-icon class="mr-1">mdi-email</v-icon>
        <span class="truncate">{{ contact.name }}</span>
      </v-card-actions>
    </v-card>
  </div>
</template>

<script lang="ts">
import Vue from "vue";

export default Vue.extend({
  name: "ContactCard",
  props: {
    contact: {
      type: Object,
      default() {
        return {};
      },
    },
  },
});
</script>
<style scoped>
.truncate {
  width: 250px;
  white-space: nowrap;
  overflow: hidden;
  text-overflow: ellipsis;
}
</style>
