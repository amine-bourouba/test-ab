<template>
  <div class="mx-15 px-5">
    <div class="mt-6 mb-4">
      <div class="d-flex">
        <v-progress-circular
          v-if="contacts.isFetching"
          indeterminate
        ></v-progress-circular>
        <div v-else-if="contacts.data" class="text-h4">
          {{ contacts.data.length }} Contact(s)
        </div>
        <v-spacer></v-spacer>
        <v-text-field label="Search..." outlined clearable></v-text-field>
      </div>
    </div>
    <v-row v-if="contacts.isFetching" class="mt-5">
      <v-col v-for="index in 32" :key="index" cols="12" sm="6" md="3">
        <v-skeleton-loader
          class="mx-auto"
          max-width="300"
          type="card"
        ></v-skeleton-loader>
      </v-col>
    </v-row>
    <v-row v-else class="mt-5">
      <v-col
        v-for="(contact, index) in contacts.data"
        :key="index"
        cols="12"
        sm="6"
        md="3"
      >
        <ContactCard :contact="contact" />
      </v-col>
    </v-row>
  </div>
</template>

<script setup lang="ts">
import { computed, reactive, ref } from "vue";
import { useFetch } from "@vueuse/core";
import ContactCard from "./ContactCard.vue";
import { get, map } from "lodash";
import { Contacts } from "../types/Contacts";

const url = ref("https://randomuser.me/api/?results=100");
const refetch = ref(false);
const { data, isFetching } = useFetch(url, { refetch }).get();

const contacts = reactive({
  isFetching,

  data: computed<Contacts>(() => {
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
