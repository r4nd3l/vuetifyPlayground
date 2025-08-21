<template>
  <v-container>
    <!-- Basic Expansion Panels -->
    <v-expansion-panels>
      <v-expansion-panel>
        <v-expansion-panel-title>
          <v-icon start>mdi-information</v-icon>
          Basic Information
        </v-expansion-panel-title>
        <v-expansion-panel-text>
          This is the basic content of the expansion panel. You can put any
          content here including text, forms, or other components.
        </v-expansion-panel-text>
      </v-expansion-panel>

      <v-expansion-panel>
        <v-expansion-panel-title>
          <v-icon start>mdi-account</v-icon>
          User Details
        </v-expansion-panel-title>
        <v-expansion-panel-text>
          <v-text-field label="Name" variant="outlined"></v-text-field>
          <v-text-field
            label="Email"
            type="email"
            variant="outlined"
          ></v-text-field>
        </v-expansion-panel-text>
      </v-expansion-panel>
    </v-expansion-panels>

    <!-- Multiple Open Panels (accordion mode) -->
    <v-expansion-panels multiple class="mt-6">
      <v-expansion-panel>
        <v-expansion-panel-title>Multiple Open Panel 1</v-expansion-panel-title>
        <v-expansion-panel-text> Content for panel 1 </v-expansion-panel-text>
      </v-expansion-panel>
      <v-expansion-panel>
        <v-expansion-panel-title>Multiple Open Panel 2</v-expansion-panel-title>
        <v-expansion-panel-text> Content for panel 2 </v-expansion-panel-text>
      </v-expansion-panel>
    </v-expansion-panels>

    <!-- Advanced Example with Actions -->
    <v-expansion-panels class="mt-6">
      <v-expansion-panel v-for="(item, i) in items" :key="i">
        <v-expansion-panel-title>
          {{ item.title }}
          <template #actions>
            <v-icon color="primary">mdi-chevron-down</v-icon>
          </template>
        </v-expansion-panel-title>
        <v-expansion-panel-text>
          <p>{{ item.content }}</p>
          <v-divider class="my-3"></v-divider>
          <div class="d-flex justify-end">
            <v-btn color="primary" variant="text" @click="editItem(item)">
              Edit
            </v-btn>
            <v-btn color="error" variant="text" @click="deleteItem(item)">
              Delete
            </v-btn>
          </div>
        </v-expansion-panel-text>
      </v-expansion-panel>
    </v-expansion-panels>

    <!-- Controlled Expansion Panels -->
    <v-expansion-panels v-model="panel" class="mt-6">
      <v-expansion-panel value="panel1">
        <v-expansion-panel-title>Controlled Panel 1</v-expansion-panel-title>
        <v-expansion-panel-text>
          Current panel: {{ panel }}
        </v-expansion-panel-text>
      </v-expansion-panel>
      <v-expansion-panel value="panel2">
        <v-expansion-panel-title>Controlled Panel 2</v-expansion-panel-title>
        <v-expansion-panel-text>
          Current panel: {{ panel }}
        </v-expansion-panel-text>
      </v-expansion-panel>
    </v-expansion-panels>
  </v-container>
</template>

<script setup>
import { ref } from "vue";

const panel = ref(null);
const items = ref([
  {
    title: "Project Overview",
    content:
      "This project involves building a comprehensive dashboard with real-time analytics and user management features.",
  },
  {
    title: "Technical Specifications",
    content:
      "Built with Vue 3, Vuetify 3, and Node.js. Includes RESTful API integration and real-time updates via WebSockets.",
  },
  {
    title: "Team Members",
    content:
      "John Doe (Lead Developer), Jane Smith (UI/UX Designer), Bob Johnson (Backend Developer)",
  },
]);

const editItem = (item) => {
  console.log("Editing:", item.title);
  // Implement edit logic
};

const deleteItem = (item) => {
  console.log("Deleting:", item.title);
  // Implement delete logic
};
</script>

<style scoped>
/* Custom styling for expansion panels */
.v-expansion-panel {
  margin-bottom: 8px;
  border-radius: 8px;
  box-shadow: 0 2px 4px rgba(0, 0, 0, 0.1);
}
</style>
