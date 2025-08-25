<template>
  <v-container class="pa-6">
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

      <v-progress-linear
        :model-value="passwordStrengthScore * 20"
        :color="
          passwordStrengthScore < 3
            ? 'red'
            : passwordStrengthScore < 5
            ? 'orange'
            : 'green'
        "
        height="10"
        class="mb-2"
      />
      <ul class="text-sm ml-2 mb-4">
        <li :class="{ 'text-green-600': passwordChecks.minLength }">
          {{ passwordChecks.minLength ? "✅" : "❌" }} At least 12 characters
        </li>
        <li :class="{ 'text-green-600': passwordChecks.uppercase }">
          {{ passwordChecks.uppercase ? "✅" : "❌" }} Uppercase letter
        </li>
        <li :class="{ 'text-green-600': passwordChecks.lowercase }">
          {{ passwordChecks.lowercase ? "✅" : "❌" }} Lowercase letter
        </li>
        <li :class="{ 'text-green-600': passwordChecks.number }">
          {{ passwordChecks.number ? "✅" : "❌" }} Number
        </li>
        <li :class="{ 'text-green-600': passwordChecks.special }">
          {{ passwordChecks.special ? "✅" : "❌" }} Special character
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

    <div class="mt-6">
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

// Checking the states
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
  await new Promise((r) => setTimeout(r, 600));
  isCheckingUsername.value = false;
  const taken = ["admin", "root", "superuser"];
  return !taken.includes(username.toLowerCase());
};

const checkEmailRegistered = async (email: string): Promise<boolean> => {
  isCheckingEmail.value = true;
  await new Promise((r) => setTimeout(r, 600));
  isCheckingEmail.value = false;
  const registered = ["ceo@company.com", "user@example.com"];
  return !registered.includes(email.toLowerCase());
};

// Password strength checks
const passwordChecks = computed(() => {
  const pwd = form.password || "";
  return {
    minLength: pwd.length >= 12,
    uppercase: /[A-Z]/.test(pwd),
    lowercase: /[a-z]/.test(pwd),
    number: /\d/.test(pwd),
    special: /[!@#$%^&*(),.?":{}|<>]/.test(pwd),
  };
});

const passwordStrengthScore = computed(() => {
  return Object.values(passwordChecks.value).filter(Boolean).length;
});

// Rules for validation
const rules = {
  required: (v: any) => !!v || v === 0 || "This field is required",

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
    if (v.length < 12) return "Must be at least 12 characters";
    if (!/[A-Z]/.test(v)) return "Must contain uppercase letter";
    if (!/[a-z]/.test(v)) return "Must contain lowercase letter";
    if (!/\d/.test(v)) return "Must contain a number";
    if (!/[!@#$%^&*(),.?\":{}|<>]/.test(v))
      return "Must contain special character";
    return true;
  },

  confirmPassword: (v: string) => v === form.password || "Passwords must match",

  terms: (v: boolean) => v || "You must accept the terms",
};

const submitForm = async (): Promise<void> => {
  const result = await formRef.value?.validate();
  if (result) {
    // Simulate server-side validation rejection
    if (form.email.endsWith("@baddomain.com")) {
      alert("Server rejected: Email domain not allowed");
      return;
    }
    alert("Form submitted successfully!");
  }
};

const resetForm = (): void => {
  formRef.value?.reset();
  formRef.value?.resetValidation();
};
</script>
