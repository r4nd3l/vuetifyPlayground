<template>
  <v-container class="pa-4">
    <v-form ref="formRef" v-model="isValid" lazy-validation>
      <v-text-field
        v-model="form.name"
        label="Name"
        :rules="[rules.required, rules.minLength]"
        required
      />

      <v-text-field
        v-model="form.email"
        label="Email"
        :rules="[rules.required, rules.email]"
        required
      />

      <v-text-field
        v-model="form.password"
        label="Password"
        type="password"
        :rules="[rules.required, rules.password]"
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
import { ref, reactive } from "vue";
import type { Ref } from "vue";

interface FormData {
  name: string;
  email: string;
  password: string;
}

// Vuetify form type (validate/reset/resetValidation exist on v-form)
interface VForm {
  validate: () => boolean;
  reset: () => void;
  resetValidation: () => void;
}

// Checking the state
const form = reactive<FormData>({
  name: "",
  email: "",
  password: "",
});

const isValid: Ref<boolean> = ref(false);
const formRef = ref<VForm | null>(null);

// Rules to validate
const rules = {
  required: (v: string) => !!v || "This field is required",
  minLength: (v: string) => (v && v.length >= 3) || "Minimum 3 characters",
  email: (v: string) => /.+@.+\..+/.test(v) || "E-mail must be valid",
  password: (v: string) =>
    (v && v.length >= 6) || "Password must be at least 6 characters",
};

const submitForm = (): void => {
  if (formRef.value?.validate()) {
    alert("Form submitted successfully!");
  }
};

const resetForm = (): void => {
  formRef.value?.reset();
  formRef.value?.resetValidation();
};
</script>
