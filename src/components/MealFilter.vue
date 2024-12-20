<template>
    <div>
      <input
        v-model="filterText"
        @input="updateFilter"
        placeholder="Filter meals by name"
        class="p-2 border rounded-md w-full mb-2"
      />
    </div>
  </template>
  
  <script setup>
  import { ref, defineEmits } from 'vue';
  
  const filterText = ref('');
  const emit = defineEmits(['filter-meals']);
  
  const updateFilter = () => {
    emit('filter-meals', filterText.value);
  };
  </script>
  