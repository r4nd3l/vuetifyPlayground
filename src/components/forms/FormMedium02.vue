<template>
  <v-app>
    <v-main>
      <v-container class="form-container">
        <v-card class="mx-auto" max-width="600" elevation="4">
          <v-card-title class="headline primary white--text">
            <v-icon large left class="mr-2">mdi-account-plus</v-icon>
            Advanced User Registration
          </v-card-title>

          <v-card-text class="pa-5">
            <v-form
              ref="form"
              v-model="valid"
              lazy-validation
              @submit.prevent="submit"
            >
              <v-row>
                <v-col cols="12" sm="6">
                  <v-text-field
                    v-model="formData.firstName"
                    :rules="nameRules"
                    label="First Name"
                    prepend-icon="mdi-account"
                    required
                    counter
                    maxlength="20"
                  ></v-text-field>
                </v-col>

                <v-col cols="12" sm="6">
                  <v-text-field
                    v-model="formData.lastName"
                    :rules="nameRules"
                    label="Last Name"
                    required
                    counter
                    maxlength="20"
                  ></v-text-field>
                </v-col>
              </v-row>

              <v-text-field
                v-model="formData.email"
                :rules="emailRules"
                label="E-mail"
                prepend-icon="mdi-email"
                required
              ></v-text-field>

              <v-row>
                <v-col cols="12" sm="6">
                  <v-text-field
                    v-model="formData.password"
                    :rules="passwordRules"
                    label="Password"
                    prepend-icon="mdi-lock"
                    :append-icon="showPassword ? 'mdi-eye' : 'mdi-eye-off'"
                    :type="showPassword ? 'text' : 'password'"
                    required
                    counter
                    @click:append="togglePasswordVisibility"
                  ></v-text-field>
                </v-col>

                <v-col cols="12" sm="6">
                  <v-text-field
                    v-model="formData.confirmPassword"
                    :rules="confirmPasswordRules"
                    label="Confirm Password"
                    prepend-icon="mdi-lock-check"
                    :type="showPassword ? 'text' : 'password'"
                    required
                  ></v-text-field>
                </v-col>
              </v-row>

              <v-text-field
                v-model="formData.phone"
                :rules="phoneRules"
                label="Phone Number"
                prepend-icon="mdi-phone"
                placeholder="(123) 456-7890"
                required
              ></v-text-field>

              <v-row>
                <v-col cols="12" sm="6">
                  <v-text-field
                    v-model="formData.age"
                    :rules="ageRules"
                    label="Age"
                    type="number"
                    prepend-icon="mdi-cake"
                    required
                  ></v-text-field>
                </v-col>

                <v-col cols="12" sm="6">
                  <v-slider
                    v-model="formData.age"
                    :rules="ageRules"
                    label="Age"
                    min="13"
                    max="100"
                    thumb-label
                    class="mt-4"
                  ></v-slider>
                </v-col>
              </v-row>

              <v-radio-group
                v-model="formData.subscription"
                :rules="subscriptionRules"
                row
                required
              >
                <template v-slot:label>
                  <div>
                    Subscription Type <v-icon small>mdi-information</v-icon>
                  </div>
                </template>
                <v-radio label="Free" value="free"></v-radio>
                <v-radio label="Pro" value="pro"></v-radio>
                <v-radio label="Enterprise" value="enterprise"></v-radio>
              </v-radio-group>

              <v-text-field
                v-if="
                  formData.subscription === 'pro' ||
                  formData.subscription === 'enterprise'
                "
                v-model="formData.promoCode"
                :rules="promoCodeRules"
                label="Promo Code"
                prepend-icon="mdi-ticket-percent"
                placeholder="Enter promo code if available"
              ></v-text-field>

              <!-- Terms Agreement -->
              <v-checkbox
                v-model="formData.terms"
                :rules="[
                  (v) => !!v || 'You must agree to the terms and conditions!',
                ]"
                label="I agree to the terms and conditions"
                required
              ></v-checkbox>

              <!-- Newsletter Opt-in -->
              <v-checkbox
                v-model="formData.newsletter"
                label="Subscribe to our newsletter"
              ></v-checkbox>

              <v-card-actions>
                <v-btn
                  :disabled="!valid"
                  color="primary"
                  class="mr-4"
                  large
                  @click="validateForm"
                >
                  <v-icon left>mdi-check</v-icon>
                  Register
                </v-btn>

                <v-btn
                  color="secondary"
                  class="mr-4"
                  @click="resetForm"
                  large
                  outlined
                >
                  <v-icon left>mdi-refresh</v-icon>
                  Reset
                </v-btn>

                <v-btn color="grey" @click="resetValidation" large text>
                  <v-icon left>mdi-close</v-icon>
                  Clear Errors
                </v-btn>
              </v-card-actions>
            </v-form>
          </v-card-text>
        </v-card>

        <v-dialog v-model="successDialog" max-width="400">
          <v-card>
            <v-card-title class="headline success white--text">
              <v-icon large left>mdi-check-circle</v-icon>
              Success!
            </v-card-title>
            <v-card-text class="pa-4">
              <p>Your registration has been completed successfully.</p>
              <v-divider class="my-3"></v-divider>
              <p>
                <strong>Name:</strong> {{ formData.firstName }}
                {{ formData.lastName }}
              </p>
              <p><strong>Email:</strong> {{ formData.email }}</p>
              <p><strong>Subscription:</strong> {{ formData.subscription }}</p>
            </v-card-text>
            <v-card-actions>
              <v-btn color="primary" block @click="successDialog = false"
                >Close</v-btn
              >
            </v-card-actions>
          </v-card>
        </v-dialog>
      </v-container>
    </v-main>
  </v-app>
</template>

<script setup lang="ts">
import { ref, reactive, computed } from "vue";
import type { Ref } from "vue";
import type { VForm } from "vuetify/components";

interface FormData {
  firstName: string;
  lastName: string;
  email: string;
  password: string;
  confirmPassword: string;
  phone: string;
  age: string | number;
  subscription: string | null;
  promoCode: string;
  terms: boolean;
  newsletter: boolean;
}

const form = ref<VForm | null>(null);
const valid = ref(false);
const showPassword = ref(false);
const successDialog = ref(false);

const formData = reactive<FormData>({
  firstName: "",
  lastName: "",
  email: "",
  password: "",
  confirmPassword: "",
  phone: "",
  age: "",
  subscription: null,
  promoCode: "",
  terms: false,
  newsletter: false,
});

// Rules for validation
const nameRules = [
  (v: string) => !!v || "Name is required",
  (v: string) => (v && v.length >= 2) || "Name must be at least 2 characters",
  (v: string) => /^[A-Za-z\s]+$/.test(v) || "Name must contain only letters",
];

const emailRules = [
  (v: string) => !!v || "E-mail is required",
  (v: string) => /.+@.+\..+/.test(v) || "E-mail must be valid",
];

const passwordRules = [
  (v: string) => !!v || "Password is required",
  (v: string) =>
    (v && v.length >= 8) || "Password must be at least 8 characters",
  (v: string) =>
    /(?=.*[A-Z])/.test(v) || "Must have at least one uppercase letter",
  (v: string) => /(?=.*\d)/.test(v) || "Must have at least one number",
];

const phoneRules = [
  (v: string) => !!v || "Phone number is required",
  (v: string) =>
    /^[\+]?[(]?[0-9]{3}[)]?[-\s\.]?[0-9]{3}[-\s\.]?[0-9]{4,6}$/.test(v) ||
    "Phone number must be valid",
];

const ageRules = [
  (v: string | number) => !!v || "Age is required",
  (v: string | number) => {
    const num = typeof v === "string" ? parseInt(v, 10) : v;
    return (
      (!isNaN(num) && num >= 13 && num <= 100) ||
      "You must be between 13 and 100 years old"
    );
  },
];

const subscriptionRules = [
  (v: string | null) => !!v || "Please select a subscription type",
];

const promoCodeRules = [
  (v: string) =>
    !v ||
    /^[A-Z0-9]{5,10}$/.test(v) ||
    "Promo code must be 5-10 alphanumeric characters",
];

// Computed property for confirm password rules
const confirmPasswordRules = computed(() => [
  (v: string) => !!v || "Please confirm your password",
  (v: string) => v === formData.password || "Passwords do not match",
]);

const togglePasswordVisibility = () => {
  showPassword.value = !showPassword.value;
};

const validateForm = async () => {
  if (form.value) {
    const { valid } = await form.value.validate();
    if (valid) {
      successDialog.value = true;
      // Submit the form data to an API
      console.log("Form submitted:", formData);
    }
  }
};

const resetForm = () => {
  if (form.value) {
    form.value.reset();
  }
};

const resetValidation = () => {
  if (form.value) {
    form.value.resetValidation();
  }
};

const submit = () => {
  validateForm();
};
</script>

<style scoped>
.form-container {
  padding-top: 40px;
  padding-bottom: 40px;
}

.headline {
  display: flex;
  align-items: center;
}

.v-card__title {
  padding: 20px;
}

.v-card__text {
  padding: 25px;
}

.v-btn {
  margin-top: 10px;
}

.v-input {
  margin-bottom: 5px;
}
</style>
