<template>
  <v-app>
    <v-main>
      <v-container class="pa-8">
        <h1 class="mb-6">Expansion Panel with Color States</h1>

        <v-expansion-panels v-model="panel" multiple>
          <!-- Panel 1 - Changes color when open -->
          <v-expansion-panel
            :class="{ 'open-panel': panel.includes(0) }"
            class="mb-4 custom-panel"
          >
            <v-expansion-panel-title
              expand-icon="mdi-chevron-down"
              collapse-icon="mdi-chevron-up"
            >
              <div class="d-flex align-center">
                <v-icon
                  :color="panel.includes(0) ? 'white' : 'primary'"
                  class="mr-3"
                  >mdi-information</v-icon
                >
                <h2
                  :class="panel.includes(0) ? 'white--text' : 'primary--text'"
                >
                  Controlled Panel 1
                </h2>
              </div>
            </v-expansion-panel-title>
            <v-expansion-panel-text class="panel-content">
              <p>
                This panel changes color when opened. The header background
                becomes blue and the text becomes white for better contrast.
              </p>
              <v-alert type="info" class="mt-3">
                Current panel state: {{ panel.includes(0) ? "OPEN" : "CLOSED" }}
              </v-alert>
            </v-expansion-panel-text>
          </v-expansion-panel>

          <!-- Panel 2 - Another example -->
          <v-expansion-panel
            :class="{ 'open-panel-2': panel.includes(1) }"
            class="mb-4 custom-panel"
          >
            <v-expansion-panel-title>
              <div class="d-flex align-center">
                <v-icon
                  :color="panel.includes(1) ? 'white' : 'green darken-2'"
                  class="mr-3"
                  >mdi-cog</v-icon
                >
                <h2
                  :class="
                    panel.includes(1)
                      ? 'white--text'
                      : 'green--text text--darken-2'
                  "
                >
                  Settings Panel
                </h2>
              </div>
            </v-expansion-panel-title>
            <v-expansion-panel-text class="panel-content">
              <p>
                This panel changes to a green color scheme when opened. Notice
                how the icon color also changes for consistency.
              </p>
              <div class="mt-3">
                <v-chip class="ma-1" color="green" text-color="white"
                  >Option 1</v-chip
                >
                <v-chip class="ma-1" color="green" text-color="white"
                  >Option 2</v-chip
                >
                <v-chip class="ma-1" color="green" text-color="white"
                  >Option 3</v-chip
                >
              </div>
            </v-expansion-panel-text>
          </v-expansion-panel>

          <!-- Panel 3 - With different color -->
          <v-expansion-panel
            :class="{ 'open-panel-3': panel.includes(2) }"
            class="mb-4 custom-panel"
          >
            <v-expansion-panel-title
              expand-icon="mdi-chevron-down"
              collapse-icon="mdi-chevron-up"
            >
              <div class="d-flex align-center">
                <v-icon
                  :color="panel.includes(2) ? 'white' : 'deep-purple accent-2'"
                  class="mr-3"
                  >mdi-alert</v-icon
                >
                <h2
                  :class="
                    panel.includes(2)
                      ? 'white--text'
                      : 'deep-purple--text text--accent-2'
                  "
                >
                  Warning Panel
                </h2>
              </div>
            </v-expansion-panel-title>
            <v-expansion-panel-text class="panel-content">
              <p>
                This panel uses a purple color scheme when opened. The color
                change provides a clear visual indicator of state.
              </p>
              <v-alert type="warning" class="mt-3">
                This is an important warning message that appears when the panel
                is expanded.
              </v-alert>
            </v-expansion-panel-text>
          </v-expansion-panel>
        </v-expansion-panels>

        <div class="mt-8 state-indicator">
          <h3>Panel States:</h3>
          <v-chip
            class="ma-1"
            :color="panel.includes(0) ? 'primary' : 'grey'"
            text-color="white"
          >
            Panel 1: {{ panel.includes(0) ? "OPEN" : "CLOSED" }}
          </v-chip>
          <v-chip
            class="ma-1"
            :color="panel.includes(1) ? 'green' : 'grey'"
            text-color="white"
          >
            Panel 2: {{ panel.includes(1) ? "OPEN" : "CLOSED" }}
          </v-chip>
          <v-chip
            class="ma-1"
            :color="panel.includes(2) ? 'deep-purple' : 'grey'"
            text-color="white"
          >
            Panel 3: {{ panel.includes(2) ? "OPEN" : "CLOSED" }}
          </v-chip>
        </div>
      </v-container>
    </v-main>
  </v-app>
</template>

<script>
export default {
  name: "ExpansionPanelDemo",
  data() {
    return {
      panel: [0], // Default open panel
    };
  },
};
</script>

<style scoped>
.custom-panel {
  transition: all 0.3s ease;
  border-radius: 8px !important;
  overflow: hidden;
  box-shadow: 0 3px 5px rgba(0, 0, 0, 0.1);
}

.open-panel :deep(.v-expansion-panel-title) {
  background-color: #1976d2; /* Primary blue */
  color: white;
}

.open-panel-2 :deep(.v-expansion-panel-title) {
  background-color: #2e7d32; /* Green */
  color: white;
}

.open-panel-3 :deep(.v-expansion-panel-title) {
  background-color: #7b1fa2; /* Deep purple */
  color: white;
}

.panel-content {
  background-color: #f7f9fc;
  padding: 20px;
}

.state-indicator {
  padding: 16px;
  background-color: #f5f5f5;
  border-radius: 8px;
}

:deep(.v-expansion-panel-title__icon) {
  color: inherit !important;
}

:deep(.v-expansion-panel-title) {
  padding: 20px;
  transition: background-color 0.3s ease;
}
</style>
