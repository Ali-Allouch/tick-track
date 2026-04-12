<template>
  <div
    class="min-h-screen bg-gray-100 dark:bg-gray-900 text-gray-900 dark:text-gray-100 p-4 transition-colors duration-200"
  >
    <div
      class="tick-track-container max-w-sm mx-auto bg-white dark:bg-gray-800 rounded-lg shadow-lg p-6"
    >
      <h1 class="text-3xl font-bold mb-6 text-center">TickTrack</h1>

      <!-- Add New Timer Toggle -->
      <div
        class="flex items-center justify-between bg-white dark:bg-gray-700 p-4 rounded-md shadow-sm mb-4 cursor-pointer"
        @click="isFormOpen = !isFormOpen"
      >
        <h2 class="text-xl font-semibold">Add New Timer</h2>
        <svg
          :class="[
            'w-6 h-6 transition-transform duration-300',
            { 'rotate-45': isFormOpen },
          ]"
          fill="currentColor"
          viewBox="0 0 20 20"
        >
          <path
            fill-rule="evenodd"
            d="M10 5a1 1 0 011 1v3h3a1 1 0 110 2h-3v3a1 1 0 11-2 0v-3H6a1 1 0 110-2h3V6a1 1 0 011-1z"
            clip-rule="evenodd"
          ></path>
        </svg>
      </div>

      <!-- New Timer Form (Collapsible) -->
      <Transition name="accordion">
        <form
          v-if="isFormOpen"
          @submit.prevent="addTimer"
          class="mb-8 space-y-4 overflow-hidden"
        >
          <div>
            <label for="eventName" class="block text-sm font-medium mb-1"
              >Event Name</label
            >
            <input
              type="text"
              id="eventName"
              v-model="newEventName"
              placeholder="e.g., Project Deadline"
              class="w-full p-2 border border-gray-300 dark:border-gray-600 rounded-md bg-gray-50 dark:bg-gray-700 text-gray-900 dark:text-gray-100 focus:ring-blue-500 focus:border-blue-500"
              required
            />
          </div>
          <div>
            <label for="targetDateTime" class="block text-sm font-medium mb-1"
              >Target Date and Time</label
            >
            <input
              type="datetime-local"
              id="targetDateTime"
              v-model="newTargetDateTime"
              :min="minDateTime"
              class="w-full p-2 border border-gray-300 dark:border-gray-600 rounded-md bg-gray-50 dark:bg-gray-700 text-gray-900 dark:text-gray-100 focus:ring-blue-500 focus:border-blue-500"
              required
            />
            <p v-if="errorMessage" class="text-red-500 text-sm mt-1">
              {{ errorMessage }}
            </p>
          </div>

          <!-- Color Picker -->
          <div class="mt-4">
            <label class="block text-sm font-medium mb-1">Pick a Color</label>
            <div
              class="color-picker-container flex bg-white w-fit space-x-2 p-1 rounded-[32px]"
            >
              <div
                v-for="preset in colorPresets"
                :key="preset.name"
                @click="selectedColor = preset.bgClass"
                :class="[
                  preset.bgClass,
                  'w-8 h-8 rounded-full cursor-pointer',
                  {
                    'ring-2 ring-offset-2 ring-blue-500':
                      selectedColor === preset.bgClass,
                  },
                ]"
                :title="preset.name"
              ></div>
            </div>
          </div>

          <button
            type="submit"
            class="w-full bg-blue-600 hover:bg-blue-700 text-white font-bold py-2 px-4 rounded-md transition-colors duration-200 cursor-pointer"
          >
            Add Timer
          </button>
        </form>
      </Transition>

      <!-- Timer List -->
      <div
        v-if="timers.length === 0"
        class="text-center text-gray-500 dark:text-gray-400"
      >
        No timers added yet. Add one above!
      </div>
      <div v-else class="space-y-4">
        <div
          v-for="timer in timers"
          :key="timer.id"
          :class="[
            'flex items-center justify-between p-4 rounded-md shadow-sm',
            timer.color,
            colorPresets.find((p) => p.bgClass === timer.color)?.textClass ||
              'text-gray-900 dark:text-gray-100',
          ]"
        >
          <div>
            <h3 class="text-lg font-semibold">{{ timer.eventName }}</h3>
            <div class="text-sm mb-2">
              Target: {{ formatDateTime(timer.targetDateTime) }}
            </div>
            <div class="text-xl font-mono">
              <span v-if="timer.remaining.total > 0">
                {{ timer.remaining.days }}d {{ timer.remaining.hours }}h
                {{ timer.remaining.minutes }}m {{ timer.remaining.seconds }}s
              </span>
              <span v-else class="text-green-600 dark:text-green-400"
                >Completed!</span
              >
            </div>
          </div>
          <button
            @click="deleteTimer(timer.id)"
            class="text-red-500 hover:text-red-700 dark:hover:text-red-400 transition-colors duration-200"
            aria-label="Delete timer"
          >
            <svg
              xmlns="http://www.w3.org/2000/svg"
              width="24"
              height="24"
              viewBox="0 0 24 24"
              fill="currentColor"
              class="icon icon-tabler icons-tabler-filled icon-tabler-trash"
            >
              <path stroke="none" d="M0 0h24v24H0z" fill="none" />
              <path
                d="M20 6a1 1 0 0 1 .117 1.993l-.117 .007h-.081l-.919 11a3 3 0 0 1 -2.824 2.995l-.176 .005h-8c-1.598 0 -2.904 -1.249 -2.992 -2.75l-.005 -.167l-.923 -11.083h-.08a1 1 0 0 1 -.117 -1.993l.117 -.007zm-10 4a1 1 0 0 0 -1 1v6a1 1 0 0 0 2 0v-6a1 1 0 0 0 -1 -1m4 0a1 1 0 0 0 -1 1v6a1 1 0 0 0 2 0v-6a1 1 0 0 0 -1 -1"
              />
              <path
                d="M14 2a2 2 0 0 1 2 2a1 1 0 0 1 -1.993 .117l-.007 -.117h-4l-.007 .117a1 1 0 0 1 -1.993 -.117a2 2 0 0 1 1.85 -1.995l.15 -.005z"
              />
            </svg>
          </button>
        </div>
      </div>
    </div>
  </div>
</template>

<script setup lang="ts">
import { ref, onMounted, onUnmounted } from "vue";

interface Timer {
  id: string;
  eventName: string;
  targetDateTime: string;
  color: string; // Add color property
  remaining: {
    total: number;
    days: number;
    hours: number;
    minutes: number;
    seconds: number;
  };
}

interface ColorPreset {
  name: string;
  bgClass: string;
  textClass: string;
}

const newEventName = ref("");
const newTargetDateTime = ref("");
const selectedColor = ref<string>("bg-blue-100"); // Default selected color
const timers = ref<Timer[]>([]);
const errorMessage = ref<string | null>(null);
const isFormOpen = ref(false); // State for form visibility
let timerInterval: number | undefined;

const colorPresets: ColorPreset[] = [
  { name: "Blue", bgClass: "bg-blue-100", textClass: "text-blue-900" },
  { name: "Red", bgClass: "bg-red-100", textClass: "text-red-900" },
  { name: "Green", bgClass: "bg-green-100", textClass: "text-green-900" },
  { name: "Purple", bgClass: "bg-purple-100", textClass: "text-purple-900" },
  { name: "Yellow", bgClass: "bg-yellow-100", textClass: "text-yellow-900" },
  { name: "Pink", bgClass: "bg-pink-100", textClass: "text-pink-900" },
  { name: "Gray", bgClass: "bg-gray-100", textClass: "text-gray-900" },
  { name: "Indigo", bgClass: "bg-indigo-100", textClass: "text-indigo-900" },
];

const getMinDateTime = () => {
  const now = new Date();
  now.setMinutes(now.getMinutes() + 1); // Set minimum to 1 minute in the future
  now.setSeconds(0); // Optional: clear seconds for cleaner input
  const year = now.getFullYear();
  const month = (now.getMonth() + 1).toString().padStart(2, "0");
  const day = now.getDate().toString().padStart(2, "0");
  const hours = now.getHours().toString().padStart(2, "0");
  const minutes = now.getMinutes().toString().padStart(2, "0");
  return `${year}-${month}-${day}T${hours}:${minutes}`;
};

const minDateTime = ref(getMinDateTime());

// Update minDateTime every minute to keep it current
setInterval(() => {
  minDateTime.value = getMinDateTime();
}, 60 * 1000); // Update every minute

// --- Storage Logic --- START
const STORAGE_KEY = "ticktrack-timers";

const getStorage = () => {
  // Check for chrome.storage first
  if (typeof chrome !== "undefined" && chrome.storage && chrome.storage.local) {
    return {
      // Wrap chrome.storage.local methods to match localStorage API for easier fallback
      getItem: (key: string) =>
        new Promise((resolve) =>
          chrome.storage.local.get(key, (items: { [key: string]: any }) =>
            resolve(items[key]),
          ),
        ),
      setItem: (key: string, value: string) =>
        new Promise<void>((resolve) => {
          chrome.storage.local.set({ [key]: value }, () => resolve());
        }),
    };
  } else if (typeof window !== "undefined" && window.localStorage) {
    // Fallback to window.localStorage
    return {
      getItem: (key: string) =>
        Promise.resolve(window.localStorage.getItem(key)),
      setItem: (key: string, value: string) =>
        Promise.resolve(window.localStorage.setItem(key, value)),
    };
  } else {
    console.warn("No storage API available. Timers will not be saved.");
    return null;
  }
};

const loadTimers = async () => {
  const storage = getStorage();
  if (storage) {
    const storedTimers = await storage.getItem(STORAGE_KEY);
    if (storedTimers) {
      timers.value = JSON.parse(storedTimers as string);
      updateAllTimers();
      sortTimers(); // Initial sort on load
    }
  }
};

const saveTimers = async () => {
  const storage = getStorage();
  if (storage) {
    const timersToStore = JSON.stringify(
      timers.value.map((timer) => ({
        id: timer.id,
        eventName: timer.eventName,
        targetDateTime: timer.targetDateTime,
        color: timer.color, // Include color in stored data
      })),
    );
    await storage.setItem(STORAGE_KEY, timersToStore);
  }
};
// --- Storage Logic --- END

const calculateRemainingTime = (target: string) => {
  const targetDate = new Date(target).getTime();
  const now = new Date().getTime();
  const total = targetDate - now;

  const seconds = Math.floor((total / 1000) % 60);
  const minutes = Math.floor((total / (1000 * 60)) % 60);
  const hours = Math.floor((total / (1000 * 60 * 60)) % 24);
  const days = Math.floor(total / (1000 * 60 * 60 * 24));

  return {
    total,
    days: days > 0 ? days : 0,
    hours: hours > 0 ? hours : 0,
    minutes: minutes > 0 ? minutes : 0,
    seconds: seconds > 0 ? seconds : 0,
  };
};

const sortTimers = () => {
  timers.value.sort((a, b) => {
    const dateA = new Date(a.targetDateTime).getTime();
    const dateB = new Date(b.targetDateTime).getTime();
    return dateA - dateB;
  });
};

const updateAllTimers = () => {
  timers.value.forEach((timer) => {
    timer.remaining = calculateRemainingTime(timer.targetDateTime);
  });
  sortTimers(); // Sort after updating remaining times
};

const addTimer = async () => {
  errorMessage.value = null; // Clear previous errors

  if (!newEventName.value || !newTargetDateTime.value) {
    errorMessage.value = "Please fill in all fields.";
    return;
  }

  const targetDate = new Date(newTargetDateTime.value);
  const now = new Date();

  // Validate that the target date is at least one minute in the future
  if (targetDate.getTime() <= now.getTime() + 60 * 1000) {
    errorMessage.value =
      "Please select a date and time at least one minute in the future.";
    return;
  }

  const newTimer: Timer = {
    id: Date.now().toString(),
    eventName: newEventName.value,
    targetDateTime: newTargetDateTime.value,
    color: selectedColor.value, // Assign selected color
    remaining: calculateRemainingTime(newTargetDateTime.value),
  };
  timers.value.push(newTimer);
  sortTimers(); // Sort after adding a new timer
  await saveTimers(); // Await saving to ensure persistence
  newEventName.value = "";
  newTargetDateTime.value = "";
  isFormOpen.value = false; // Auto-collapse the form after adding a timer
};

const deleteTimer = (id: string) => {
  timers.value = timers.value.filter((timer) => timer.id !== id);
  sortTimers(); // Sort after deleting a timer
  saveTimers();
};

const formatDateTime = (dateTimeString: string) => {
  const date = new Date(dateTimeString);
  return date.toLocaleString();
};

onMounted(() => {
  loadTimers();
  timerInterval = setInterval(updateAllTimers, 1000);
});

onUnmounted(() => {
  if (timerInterval) {
    clearInterval(timerInterval);
  }
});
</script>

<style>
html,
body,
#app {
  min-height: 100vh;
  margin: 0;
}

.tick-track-container {
  background: #08a3b9;
  background: linear-gradient(
    90deg,
    rgb(219 250 255) 0%,
    rgb(228 255 204) 100%
  );
}

.accordion-enter-active,
.accordion-leave-active {
  transition: all 0.3s ease-in-out;
  max-height: 500px;
}
.accordion-enter-from,
.accordion-leave-to {
  max-height: 0;
  opacity: 0;
}
</style>
