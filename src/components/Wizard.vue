<script setup>
import { ref, watch, watchEffect } from "vue";
import "@/assets/tailwind.scss";

const usernameInput = ref("");
const emailInput = ref("");
const errors = ref([]);
const step = defineModel("step", { default: 0 });

const disabledBackButton = ref(false);
const disableNextButton = ref(false);

watchEffect(() => {
  if (step.value === 0) {
    disabledBackButton.value = true;
  } else {
    disabledBackButton.value = false;
  }

  if (step.value === 2) {
    disableNextButton.value = true;
  } else {
    disableNextButton.value = false;
  }
});

const handlePrevStep = () => {
  switch (step.value) {
    case 0:
      step.value = 0;
      break;
    case 1:
      step.value = 0;
      break;
    case 2:
      step.value = 1;
      break;
  }
};

const handleNextStep = () => {
  errors.value = [];
  switch (step.value) {
    case 0:
      const validCharRegex = /^[a-zA-Z0-9_]+$/;
      const username = usernameInput.value;
      if (
        !validCharRegex.test(username) ||
        username.length < 4 ||
        username.length > 16
      ) {
        errors.value.push({ date: new Date(), message: "Invalid Username." });
      } else {
        step.value = 1;
      }
      break;
    case 1:
      const validEmailRegex = /^[^\s@]+@[^\s@]+\.[^\s@]+$/;
      const email = emailInput.value;
      if (!validEmailRegex.test(email)) {
        errors.value.push({
          date: new Date(),
          message: `Invalid email address.`,
        });
      } else {
        step.value = 2;
      }
      break;
    default:
      break;
  }
};
</script>
<template>
  <header
    class="w-full flex h-16 bg-gray-800 border-gray-700 border-b items-center justify-center text-xl"
  >
    <div>
      Step:
      <span v-show="step === 0">username</span>
      <span v-show="step === 1">email</span>
      <span v-show="step === 2">review</span>
    </div>
  </header>
  <main>
    <div
      class="flex flex-col p-4 border-gray-700 border bg-gray-800 rounded-lg w-full md:w-96 min-h-64"
    >
      <div class="w-full h-full flex flex-col flex-1 justify-center">
        <div class="my-auto px-2">
          <div v-if="step === 0" class="h-full">
            <label for="username" class="pb-2 flex text-sm font-bold text-white"
              >Username:</label
            >
            <input
              name="username"
              id="username"
              class="user-input"
              placeholder="mohammad_seddigh"
              required="1"
              v-model="usernameInput"
            />
          </div>
          <div v-if="step === 1" class="h-full">
            <label for="email" class="pb-2 flex text-sm font-bold text-white"
              >Email:</label
            >
            <!-- cypress has a bug if type of this field is 'email'. it cant Type :))) -->
            <input
              name="email"
              id="email"
              class="user-input"
              placeholder="mohammad@gmail.com"
              required=""
              v-model="emailInput"
            />
          </div>
          <div
            v-if="step === 2"
            class="h-full flex flex-col gap-4 font-bold text-lg"
          >
            <span> Step: review </span>
            <span>Username: {{ usernameInput }}</span>
            <span>Email: {{ emailInput }}</span>
          </div>
          <div class="min-h-8 p-1">
            <div class="text-red-400 transition delay-75 relative">
              <TransitionGroup name="fade">
                <span
                  class="absolute"
                  v-for="error in errors"
                  :key="error.date"
                >
                  {{ error.message }}
                </span>
              </TransitionGroup>
            </div>
          </div>
        </div>
        <div
          class="footer mt-auto flex border-t border-gray-700 p-2 pt-4 justify-between"
        >
          <div>
            <button
              id="btn-prev"
              @click="handlePrevStep"
              :disabled="disabledBackButton"
              class="action-button"
            >
              Prev
            </button>
          </div>
          <div>
            <button
              :disabled="disableNextButton"
              id="btn-next"
              @click="handleNextStep"
              class="action-button"
            >
              Next
            </button>
          </div>
        </div>
      </div>
    </div>
  </main>
</template>

<style scoped lang="scss">
main {
  display: flex;
  flex-direction: column;
  min-height: 80vh;
  align-items: center;
  justify-content: center;
  padding: 30px;
}

.action-button {
  @apply px-10 py-2 font-bold hover:bg-gray-700 transition border rounded-lg border-gray-600 bg-gray-800;

  &:disabled {
    opacity: 0.4;
  }
}

.user-input {
  @apply text-gray-300 bg-gray-700 border border-gray-600 rounded-lg w-full p-3 bg-gray-700 border-gray-600 placeholder-gray-500;
}

.fade-enter-active {
  transition: all 0.5s ease-out;
}

.fade-leave-active {
  transition: all 0.1s cubic-bezier(1, 0.5, 0.8, 1);
}

.fade-enter-from,
.fade-leave-to {
  opacity: 0;
}
</style>