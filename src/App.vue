<script setup lang="ts">
import TheHeader from "@/components/TheHeader.vue";
import EntryEditor from "./components/EntryEditor.vue";
import EntryCard from "@/components/EntryCard.vue";
import { provide, inject, reactive } from "vue";
import type User from "./types/User";
import type Entry from "./types/Entry";
import { userInjectionKey } from "./injectionKeys";

const user: User = reactive({
  id: 1,
  username: "Acke",
  settings: [],
});

provide(userInjectionKey, user);

const entries: Array<Entry> = reactive([]);

console.log(user.username);

const handleCreateEntry = (entry: Entry) => {
  console.log(entry);
  entries.unshift(entry);
};
</script>

<template>
  <main class="container m-auto p-10">
    <TheHeader />
    <EntryEditor @@create="handleCreateEntry" />
    <ul>
      <li v-for="entry in entries" :key="entry.id">
        <EntryCard :entry="entry" />
      </li>
    </ul>
  </main>
</template>
