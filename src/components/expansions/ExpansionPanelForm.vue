<template>
  <v-container>
    <v-expansion-panels>
      <v-expansion-panel>
        <v-expansion-panel-title>
          <v-icon start>mdi-account-plus</v-icon>
          User Registration Form
        </v-expansion-panel-title>
        <v-expansion-panel-text>
          <v-form @submit.prevent="submitForm" ref="form">
            <v-row>
              <v-col cols="12" md="6">
                <v-text-field
                  v-model="user.firstName"
                  label="First Name"
                  :rules="[requiredRule]"
                  variant="outlined"
                ></v-text-field>
              </v-col>
              <v-col cols="12" md="6">
                <v-text-field
                  v-model="user.lastName"
                  label="Last Name"
                  :rules="[requiredRule]"
                  variant="outlined"
                ></v-text-field>
              </v-col>
              <v-col cols="12">
                <v-text-field
                  v-model="user.email"
                  label="Email"
                  type="email"
                  :rules="[requiredRule, emailRule]"
                  variant="outlined"
                ></v-text-field>
              </v-col>
              <v-col cols="12">
                <v-select
                  v-model="user.role"
                  :items="roles"
                  label="Role"
                  :rules="[requiredRule]"
                  variant="outlined"
                ></v-select>
              </v-col>
            </v-row>

            <v-divider class="my-4"></v-divider>

            <div class="d-flex justify-end">
              <v-btn color="primary" type="submit" :loading="loading">
                Register User
              </v-btn>
            </div>
          </v-form>
        </v-expansion-panel-text>
      </v-expansion-panel>
    </v-expansion-panels>
  </v-container>
</template>

<script setup>
import { ref } from "vue";

const form = ref(null);
const loading = ref(false);
const user = ref({
  firstName: "",
  lastName: "",
  email: "",
  role: "",
});

const roles = ["Admin", "User", "Guest", "Moderator"];

const requiredRule = (value) => !!value || "This field is required";
const emailRule = (value) => /.+@.+\..+/.test(value) || "Invalid email format";

const submitForm = async () => {
  const { valid } = await form.value.validate();

  if (valid) {
    loading.value = true;
    try {
      // Simulate API call
      await new Promise((resolve) => setTimeout(resolve, 1000));
      console.log("User registered:", user.value);
      alert("User registered successfully!");
    } catch (error) {
      console.error("Error:", error);
    } finally {
      loading.value = false;
    }
  }
};
</script>
