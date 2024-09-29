<template>
  <section class="filters mb-4 md:mb-4">
    <div class="flex flex-wrap space-x-2">
      <label class="mr-2">Sort By:</label>
      <button 
          class="btn" 
        :class="{'bg-blue-500 text-white': sortBy === 'twubricScore'}"
        @click="toggleSort('twubricScore')">
        Tubric Score
      </button>
      <button 
        class="btn" 
        :class="{'bg-blue-500 text-white': sortBy === 'friends'}"
        @click="toggleSort('friends')">
        Friends
      </button>
      <button 
        class="btn" 
        :class="{'bg-blue-500 text-white': sortBy === 'influence'}"
        @click="toggleSort('influence')">
        Influence
      </button>
      <button 
        class="btn" 
        :class="{'bg-blue-500 text-white': sortBy === 'chirpiness'}"
        @click="toggleSort('chirpiness')">
        Chirpiness
      </button>
    </div>
  </section>
</template>

<script>
export default {
  props: ['sortBy'],
  emits: ['updateSort'],
  methods: {
    toggleSort(field) {
      this.$emit('updateSort', field);
    }
  }
};
</script>

<style scoped>
.btn {
  border: 1px solid #007bff;
  background-color: white;
  color: #007bff;
  padding: 0.5rem 1rem;
  border-radius: 0.375rem;
  cursor: pointer;
  margin-bottom: 6px;
  transition: background-color 0.3s;
}
.btn:hover {
  background-color: #007bff;
  color: white;
}
</style>
