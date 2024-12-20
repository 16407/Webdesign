<template>
  <div class="bg-gray-100 min-h-screen">
    <!-- Header -->
    <header class="text-center py-8 bg-blue-500 text-white">
      <h1 class="text-4xl font-bold">Healthy Lifestyle & Meal Planning</h1>
      <p class="text-lg mt-2">Manage your meals and stay on track!</p>
    </header>

    <!-- Add Meal Section -->
    <div class="max-w-4xl mx-auto mt-6">
      <h2 class="text-2xl font-bold mb-4 text-gray-700">Add a New Meal</h2>
      <AddMeal @add-meal="addMeal" />
    </div>

    <!-- Filters -->
    <div class="max-w-4xl mx-auto mt-6 mb-4 flex gap-4">
      <!-- Search by Meal Name -->
      <input
        v-model="searchText"
        placeholder="Filter by meal name"
        class="p-2 border rounded-md w-full"
      />

      <!-- Filter by Category -->
      <select
        v-model="selectedCategory"
        class="p-2 border rounded-md"
      >
        <option value="">All Categories</option>
        <option value="Breakfast">Breakfast</option>
        <option value="Lunch">Lunch</option>
        <option value="Dinner">Dinner</option>
      </select>
    </div>

    <!-- Meal List Section -->
    <div class="max-w-4xl mx-auto mt-6">
      <h2 class="text-2xl font-bold mb-4 text-gray-700">Meal List</h2>
      <MealList
        :meals="filteredMeals"
        @delete-meal="deleteMeal"
        @update-meal="updateMeal"
      />
    </div>
  </div>
</template>

<script setup>
import { ref, computed, onMounted, watch } from "vue";
import AddMeal from "./components/AddMeal.vue";
import MealList from "./components/MealList.vue";

// Reactive state for meals and filters
const meals = ref([]);
const searchText = ref("");
const selectedCategory = ref("");

// Load meals from localStorage when the app starts
onMounted(() => {
  const savedMeals = localStorage.getItem("meals");
  if (savedMeals) {
    meals.value = JSON.parse(savedMeals);
  }
});

// Watch for changes in meals and persist to localStorage
watch(
  meals,
  (newMeals) => {
    localStorage.setItem("meals", JSON.stringify(newMeals));
  },
  { deep: true }
);

// Add a new meal
const addMeal = (meal) => {
  meals.value.push(meal);
};

// Delete a meal
const deleteMeal = (index) => {
  meals.value.splice(index, 1);
};

// Update an existing meal
const updateMeal = (index, updatedMeal) => {
  meals.value[index] = updatedMeal;
};

// Filter meals dynamically based on search text and selected category
const filteredMeals = computed(() => {
  return meals.value.filter((meal) => {
    const matchesName = meal.name
      .toLowerCase()
      .includes(searchText.value.toLowerCase());
    const matchesCategory =
      !selectedCategory.value || meal.category === selectedCategory.value;
    return matchesName && matchesCategory;
  });
});
</script>

<style scoped>
/* Optional: Scoped styles for minor tweaks */
</style>
