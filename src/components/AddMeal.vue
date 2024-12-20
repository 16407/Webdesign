<template>
  <form @submit.prevent="submitMeal" class="bg-white p-6 rounded-lg shadow-md">
    <!-- Meal Name -->
    <div class="mb-4">
      <label class="block text-gray-700 font-medium mb-1">Meal Name</label>
      <input
        v-model="name"
        type="text"
        placeholder="Enter meal name"
        class="p-2 w-full border rounded-md focus:outline-none focus:ring-2 focus:ring-blue-500"
        required
      />
    </div>

    <!-- Image Upload -->
    <div class="mb-4">
      <label class="block text-gray-700 font-medium mb-1">Meal Image</label>
      <input
        type="file"
        @change="uploadImage"
        class="p-2 w-full border rounded-md focus:outline-none focus:ring-2 focus:ring-blue-500"
        accept="image/*"
        required
      />
    </div>

    <!-- Description -->
    <div class="mb-4">
      <label class="block text-gray-700 font-medium mb-1">Meal Description</label>
      <textarea
        v-model="description"
        rows="3"
        placeholder="Enter meal description"
        class="p-2 w-full border rounded-md focus:outline-none focus:ring-2 focus:ring-blue-500"
        required
      ></textarea>
    </div>

    <!-- Category Dropdown -->
    <div class="mb-4">
      <label class="block text-gray-700 font-medium mb-1">Meal Category</label>
      <select
        v-model="category"
        class="p-2 w-full border rounded-md focus:outline-none focus:ring-2 focus:ring-blue-500"
        required
      >
        <option value="" disabled>Select Category</option>
        <option value="Breakfast">Breakfast</option>
        <option value="Lunch">Lunch</option>
        <option value="Dinner">Dinner</option>
      </select>
    </div>

    <!-- Submit Button -->
    <button
      type="submit"
      class="w-full bg-blue-500 hover:bg-blue-600 text-white px-4 py-2 rounded-md transition duration-300"
    >
      Add Meal
    </button>

    <!-- Image Preview -->
    <div v-if="previewImage" class="mt-4">
      <p class="text-gray-600 font-medium mb-2">Image Preview:</p>
      <img
        :src="previewImage"
        alt="Preview"
        class="w-full h-40 object-cover rounded-md border"
      />
    </div>
  </form>
</template>

<script setup>
import { ref } from "vue";

const emit = defineEmits(["add-meal"]);

// Reactive form fields
const name = ref("");
const description = ref("");
const category = ref("");
const image = ref("");
const previewImage = ref("");

// Handle file upload and image preview
const uploadImage = (event) => {
  const file = event.target.files[0];
  if (file) {
    previewImage.value = URL.createObjectURL(file); // Preview the uploaded image
    image.value = previewImage.value; // Save the image link
  }
};

// Emit the new meal data to the parent component
const submitMeal = () => {
  emit("add-meal", {
    name: name.value,
    description: description.value,
    category: category.value,
    image: image.value,
  });

  // Reset the form fields
  name.value = "";
  description.value = "";
  category.value = "";
  image.value = "";
  previewImage.value = "";
};
</script>

<style scoped>
/* Optional: Scoped styling to add small tweaks */
label {
  font-size: 0.9rem;
}
</style>
