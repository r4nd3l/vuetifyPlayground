<template>
  <v-container>
    <h2 class="text-h5 mb-4">Frequently Asked Questions</h2>

    <v-expansion-panels variant="popout">
      <v-expansion-panel v-for="(faq, index) in faqs" :key="index">
        <v-expansion-panel-title
          expand-icon="mdi-plus"
          collapse-icon="mdi-minus"
        >
          {{ faq.question }}
        </v-expansion-panel-title>
        <v-expansion-panel-text>
          {{ faq.answer }}
        </v-expansion-panel-text>
      </v-expansion-panel>
    </v-expansion-panels>
  </v-container>
</template>

<script setup>
import { ref } from "vue";
const faqs = ref([
  {
    question: "How do I reset my password?",
    answer:
      'Go to the settings page and click on "Reset Password". You will receive an email with instructions.',
  },
  {
    question: "What payment methods do you accept?",
    answer: "We accept credit cards, PayPal, and bank transfers.",
  },
  {
    question: "How can I contact support?",
    answer:
      "You can reach our support team via email at support@example.com or through our contact form.",
  },
  {
    question: "Is there a mobile app available?",
    answer:
      "Yes, we have mobile apps available for both iOS and Android devices.",
  },
]);
</script>
