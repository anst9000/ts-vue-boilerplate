<script lang="ts" setup>
import EmojiField from "@/components/EmojiField.vue";
import ArrowCircleRight from "@/assets/icons/arrow-circle-right.svg";
import type Emoji from "@/types/Emoji";
import { ref, inject, computed, onMounted } from "vue";
import type Entry from "@/types/Entry";
import { userInjectionKey } from "../injectionKeys"
import { MAX_CHARS, ANONYMOUS } from '@/constants';

// DATA
const user = inject(userInjectionKey)
const body = ref("");
const emoji = ref<Emoji | null>(null);
const charCount = computed(() => body.value.length);

// TEMPLATE REFS
const textarea = ref<HTMLTextAreaElement | null>(null);
onMounted(() => textarea.value?.focus());

// EVENTS
const emit = defineEmits<{
  (e: "@create", entry: Entry): void;
}>();

// METHODS
const handleTextInput = (e: Event) => {
  const textarea = e.target as HTMLTextAreaElement;
  if (textarea.value.length < MAX_CHARS) {
    body.value = textarea.value;
  } else {
    body.value = textarea.value = textarea.value.substring(0, MAX_CHARS);
  }
};

const handleSubmit = () => {
  console.log("form submitted");
  emit("@create", {
    id: Math.random(),
    body: body.value,
    emoji: emoji.value,
    userId: 1,
    createdAt: new Date(),
  });

  body.value = "";
  emoji.value = null;
};
</script>

<template>
  <form class="entry-form" @submit.prevent="handleSubmit">
    <textarea
      :value="body"
      ref="textarea"
      @keyup="handleTextInput"
      :placeholder="`New Journal Entry for ${user?.username || ANONYMOUS }`"
    ></textarea>
    <EmojiField v-model="emoji" />

    <div class="entry-form-footer">
      <span>{{ charCount }} / {{ MAX_CHARS }}</span>
      <button>Remember <ArrowCircleRight width="20" /></button>
    </div>
  </form>
</template>
@/constants