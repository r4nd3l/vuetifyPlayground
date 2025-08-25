<template>
  <v-container class="pa-4">
    <v-form ref="formRef" v-model="isValid" lazy-validation>
      <v-text-field
        v-model="form.username"
        label="Username"
        :rules="[rules.required, rules.username]"
        :loading="isCheckingUsername"
        hint="At least 4 chars, unique"
        persistent-hint
        required
      />

      <v-text-field
        v-model="form.email"
        label="Email"
        :rules="[rules.required, rules.email]"
        :loading="isCheckingEmail"
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

      <ul class="text-sm ml-2 mt-1 text-gray-600">
        <li :class="{ 'text-green-600': passwordChecks.minLength }">
          {{ passwordChecks.minLength ? "✅" : "❌" }} At least 8 characters
        </li>
        <li :class="{ 'text-green-600': passwordChecks.uppercase }">
          {{ passwordChecks.uppercase ? "✅" : "❌" }} Contains uppercase letter
        </li>
        <li :class="{ 'text-green-600': passwordChecks.lowercase }">
          {{ passwordChecks.lowercase ? "✅" : "❌" }} Contains lowercase letter
        </li>
        <li :class="{ 'text-green-600': passwordChecks.number }">
          {{ passwordChecks.number ? "✅" : "❌" }} Contains number
        </li>
        <li :class="{ 'text-green-600': passwordChecks.special }">
          {{ passwordChecks.special ? "✅" : "❌" }} Contains special character
        </li>
      </ul>

      <v-text-field
        v-model="form.confirmPassword"
        label="Confirm Password"
        type="password"
        :rules="[rules.required, rules.confirmPassword]"
        required
      />

      <v-checkbox
        v-model="form.terms"
        :rules="[rules.terms]"
        label="I agree to the Terms & Conditions"
        required
      />

      <v-btn color="primary" class="mt-4" @click="submitForm"> Submit </v-btn>
      <v-btn color="secondary" class="mt-4 ml-2" @click="resetForm">
        Reset
      </v-btn>
    </v-form>

    <div class="mt-4">
      <strong>Form Data:</strong>
      <pre>{{ form }}</pre>
    </div>
  </v-container>
</template>

<script setup lang="ts">
import { ref, reactive, computed } from "vue";
import type { Ref } from "vue";

interface FormData {
  username: string;
  email: string;
  age: number | null;
  password: string;
  confirmPassword: string;
  terms: boolean;
}

interface VForm {
  validate: () => Promise<boolean> | boolean;
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
  terms: false,
});

const isValid: Ref<boolean> = ref(false);
const formRef = ref<VForm | null>(null);

const isCheckingUsername = ref(false);
const isCheckingEmail = ref(false);

// Mock API validators
const checkUsernameAvailability = async (
  username: string
): Promise<boolean> => {
  isCheckingUsername.value = true;
  await new Promise((r) => setTimeout(r, 800)); // simulate API delay
  isCheckingUsername.value = false;

  const taken = ["admin", "test", "root"];
  return !taken.includes(username.toLowerCase());
};

const checkEmailRegistered = async (email: string): Promise<boolean> => {
  isCheckingEmail.value = true;
  await new Promise((r) => setTimeout(r, 800));
  isCheckingEmail.value = false;

  const registered = ["user@example.com", "john@doe.com"];
  return !registered.includes(email.toLowerCase());
};

// Password strength checks ---
const passwordChecks = computed(() => {
  const pwd = form.password;
  return {
    minLength: pwd.length >= 8,
    uppercase: /[A-Z]/.test(pwd),
    lowercase: /[a-z]/.test(pwd),
    number: /\d/.test(pwd),
    special: /[!@#$%^&*(),.?":{}|<>]/.test(pwd),
  };
});

// Rules for validation
const rules = {
  required: (v: string | number | boolean | null) =>
    !!v || v === 0 || "This field is required",

  username: async (v: string) => {
    if (!v || v.length < 4) return "At least 4 characters";
    if (!/^[a-zA-Z0-9_]+$/.test(v)) return "Only letters, numbers, underscores";
    const available = await checkUsernameAvailability(v);
    return available || "This username is already taken";
  },

  email: async (v: string) => {
    if (!/.+@.+\..+/.test(v)) return "E-mail must be valid";
    const available = await checkEmailRegistered(v);
    return available || "This email is already registered";
  },

  age: (v: number | null) => {
    if (v === null) return "Age is required";
    return (v >= 18 && v <= 100) || "Age must be between 18 and 100";
  },

  password: (v: string) => {
    if (v.length < 8) return "At least 8 characters";
    if (!/[A-Z]/.test(v)) return "Must contain uppercase letter";
    if (!/[a-z]/.test(v)) return "Must contain lowercase letter";
    if (!/\d/.test(v)) return "Must contain a number";
    if (!/[!@#$%^&*(),.?":{}|<>]/.test(v))
      return "Must contain special character";
    return true;
  },

  confirmPassword: (v: string) => v === form.password || "Passwords must match",

  terms: (v: boolean) => v || "You must accept the terms",
};

const submitForm = async (): Promise<void> => {
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
