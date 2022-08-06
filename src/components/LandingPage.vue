<template>
  <div>
    <v-row class="pa-4">
      <v-col
        v-for="(contact, index) in contacts.data"
        :key="index"
        cols="12"
        sm="6"
        md="3"
      >
        <v-skeleton-loader
          v-if="contacts.isFetching"
          class="mx-auto"
          max-width="300"
          type="card"
        ></v-skeleton-loader>
        <ContactCard v-else :contact="contact" />
      </v-col>
    </v-row>
  </div>
</template>

<script setup lang="ts">
import { computed, reactive, ref } from "vue";
import { useFetch } from "@vueuse/core";
import ContactCard from "./ContactCard.vue";
import { get, map } from "lodash";

const url = ref("https://randomuser.me/api/?results=100");
const refetch = ref(false);
const { data, isFetching } = useFetch(url, { refetch }).get();

const contacts = reactive({
  isFetching,

  data: computed(() => {
    try {
      const tempData = JSON.parse(data.value as string);
      if (tempData.results) {
        return map(tempData.results, (contact) => {
          const name = `${get(contact, "name.title", null)}.${get(
            contact,
            "name.first",
            null
          )} ${get(contact, "name.last", null)}`;
          return {
            name: name,
            src: get(contact, "picture.large", null),
          };
        });
      } else {
        return [];
      }
    } catch (e) {
      return null;
    }
  }),
});
</script>
