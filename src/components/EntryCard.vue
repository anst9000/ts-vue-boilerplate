<script lang="ts" setup>
import DateDisplay from "./DateDisplay.vue";
import { userInjectionKey } from "@/injectionKeys";
import UseEmojis from "@/composables/UseEmojis";
const { findEmoji } = UseEmojis();
import type Entry from "@/types/Entry";
import { ANONYMOUS } from "@/constants";
import { inject } from "vue";

defineProps<{
  entry: Entry;
}>();

const user = inject(userInjectionKey);
</script>

<template>
  <div class="entry-card">
    <div class="entry-card-body">
      <component width="75" :is="findEmoji(entry.emoji)"></component>
      <div class="entry-text">{{ entry.body }}</div>
    </div>
    <div class="entry-footer">
      <DateDisplay :date="entry.createdAt" class="mr-2" />
      |
      <span class="ml-2">{{ user?.username || ANONYMOUS }}</span>
    </div>
  </div>
</template>
