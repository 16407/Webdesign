<template>
  <div class="bg-white rounded-lg shadow-md p-4">
    <img
      :src="meal.image"
      alt="Meal"
      class="h-32 w-full object-cover rounded-md mb-4"
    />

    <!-- Display or Edit Form -->
    <div v-if="!isEditing">
      <h3 class="text-xl font-semibold">{{ meal.name }}</h3>
      <p class="text-gray-600">{{ meal.description }}</p>
      <div class="flex gap-2 mt-4">
        <button
          @click="isEditing = true"
          class="bg-yellow-500 text-white px-4 py-2 rounded hover:bg-yellow-600"
        >
          Edit
        </button>
        <button
          @click="$emit('delete-meal')"
          class="bg-red-500 text-white px-4 py-2 rounded hover:bg-red-600"
        >
          Delete
        </button>
      </div>
    </div>

    <!-- Edit Form -->
    <div v-else>
      <input
        v-model="editedMeal.name"
        placeholder="Meal Name"
        class="p-2 border rounded-md w-full mb-2"
      />
      <textarea
        v-model="editedMeal.description"
        placeholder="Meal Description"
        class="p-2 border rounded-md w-full mb-2"
      ></textarea>
      <button
        @click="saveChanges"
        class="bg-green-500 text-white px-4 py-2 rounded hover:bg-green-600"
      >
        Save
      </button>
      <button
        @click="cancelEdit"
        class="bg-gray-400 text-white px-4 py-2 rounded hover:bg-gray-500 ml-2"
      >
        Cancel
      </button>
    </div>
  </div>
</template>

<script setup>
import { ref } from "vue";

const props = defineProps({ meal: Object });
const emit = defineEmits(["delete-meal", "update-meal"]);

const isEditing = ref(false);
const editedMeal = ref({ ...props.meal });

const saveChanges = () => {
  emit("update-meal", editedMeal.value);
  isEditing.value = false;
};

const cancelEdit = () => {
  editedMeal.value = { ...props.meal };
  isEditing.value = false;
};
</script>
