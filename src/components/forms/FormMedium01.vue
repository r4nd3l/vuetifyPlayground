<template>
  <v-container class="pa-4">
    <v-form ref="formRef" v-model="isValid" lazy-validation>
      <v-text-field
        v-model="form.username"
        label="Username"
        :rules="[rules.required, rules.username]"
        :loading="isCheckingUsername"
        hint="Must be unique, at least 4 chars"
        persistent-hint
        required
      />

      <v-text-field
        v-model="form.email"
        label="Email"
        :rules="[rules.required, rules.email]"
        required
      />

      <v-text-field
        v-model="form.age"
        label="Age"
        type="number"
        :rules="[rules.required, rules.age]"
        required
      />

      <v-text-field
        v-model="form.password"
        label="Password"
        type="password"
        :rules="[rules.required, rules.password]"
        required
      />

      <v-text-field
        v-model="form.confirmPassword"
        label="Confirm Password"
        type="password"
        :rules="[rules.required, rules.confirmPassword]"
        required
      />

      <v-btn color="primary" class="mt-4" @click="submitForm"> Submit </v-btn>
      <v-btn color="secondary" class="mt-4 ml-2" @click="resetForm">
        Reset
      </v-btn>
    </v-form>

    <!-- Debug the output -->
    <div class="mt-4">
      <strong>Form Data:</strong>
      <pre>{{ form }}</pre>
    </div>
  </v-container>
</template>

<script setup lang="ts">
import { ref, reactive } from "vue";
import type { Ref } from "vue";

interface FormData {
  username: string;
  email: string;
  age: number | null;
  password: string;
  confirmPassword: string;
}

interface VForm {
  validate: () => boolean;
  reset: () => void;
  resetValidation: () => void;
}

// Checking the state
const form = reactive<FormData>({
  username: "",
  email: "",
  age: null,
  password: "",
  confirmPassword: "",
});

const isValid: Ref<boolean> = ref(false);
const formRef = ref<VForm | null>(null);

const isCheckingUsername = ref(false);

// Async validator with helper
const checkUsernameAvailability = async (
  username: string
): Promise<boolean> => {
  isCheckingUsername.value = true;
  await new Promise((r) => setTimeout(r, 800)); // simulate API delay
  isCheckingUsername.value = false;

  // Fake taken usernames
  const taken = ["admin", "test", "root"];
  return !taken.includes(username.toLowerCase());
};

// Rules to validate
const rules = {
  required: (v: string | number | null) =>
    !!v || v === 0 || "This field is required",

  username: async (v: string) => {
    if (!v || v.length < 4) return "At least 4 characters";
    if (!/^[a-zA-Z0-9_]+$/.test(v)) return "Only letters, numbers, underscores";
    const available = await checkUsernameAvailability(v);
    return available || "This username is already taken";
  },

  email: (v: string) => /.+@.+\..+/.test(v) || "E-mail must be valid",

  age: (v: number | null) => {
    if (v === null) return "Age is required";
    return (v >= 18 && v <= 100) || "Age must be between 18 and 100";
  },

  password: (v: string) =>
    (v && v.length >= 6) || "Password must be at least 6 characters",

  confirmPassword: (v: string) => v === form.password || "Passwords must match",
};

const submitForm = async (): Promise<void> => {
  // v-form with async rules to use `await`
  const result = await formRef.value?.validate();

  if (result) {
    alert("Form submitted successfully!");
  }
};

const resetForm = (): void => {
  formRef.value?.reset();
  formRef.value?.resetValidation();
};
</script>
