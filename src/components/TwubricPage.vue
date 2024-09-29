<template>
  <div class="container mx-auto p-4">
    <h1 class="text-3xl font-bold text-center mb-4">Tubric Data</h1>

    <section class="flex flex-col md:flex-col justify-between mb-4">
      <SortComponent 
        :sortBy="sortBy" 
         @updateSort="toggleSort" 
      />
      <FilterComponent 
        :startDate="startDate" 
        :endDate="endDate" 
        @updateStartDate="(val) => startDate = val"
        @updateEndDate="(val) => endDate = val"
      />
       </section>

    <main class="grid grid-cols-1 md:grid-cols-2 lg:grid-cols-3 gap-4">
      <section 
        v-if="filteredAndSortedData.length === 0" 
        class="col-span-full text-center p-4 border border-gray-300 rounded-lg bg-red-50">
        <p class="text-red-600 font-bold">No data found</p>
      </section>

      <section 
        v-for="user in filteredAndSortedData" 
        :key="user.twitter_handle" 
        class="border py-4 rounded-lg transition-transform transform hover:scale-105 bg-blue-50">
        <p class="font-bold">{{ user.twitter_handle }}</p>
        <div class="grid grid-cols-2 gap-2 mb-2">
          <p>{{ user.fullname }}</p>
          <p>{{ user.twubric.total }}</p>
        </div>
        <hr class="border border-gray-300 my-2">
        
        <div class="grid grid-cols-3 gap-2 mb-2">
          <p>Friends: {{ user.twubric.friends }}</p>
          <p>Influence: {{ user.twubric.influence }}</p>
          <p>Chirpiness: {{ user.twubric.chirpiness }}</p>
        </div>
        <hr class="border border-gray-300 my-2">
        
        <div class="grid grid-cols-2 gap-2 mb-2">
          <p>Joined: {{ formatDate(user.join_date) }}</p>
          <button 
            class="bg-red-500 text-white px-2 py-1 rounded"
            @click="removeUser(user.uid)">
            Remove
          </button>
        </div>
      </section>
    </main>
  </div>
</template>

<script>
import { ref, computed, onMounted } from 'vue';
import axios from 'axios';
import SortComponent from './SortComponent.vue';
import FilterComponent from './FilterComponent.vue';

export default {
  components: {
    SortComponent,
    FilterComponent
  },
  setup() {
    const users = ref([]);
    const sortBy = ref('twubricScore');
    const startDate = ref(null);
    const endDate = ref(null);

    const fetchData = async () => {
      const response = await axios.get('https://gist.githubusercontent.com/pandemonia/21703a6a303e0487a73b2610c8db41ab/raw/82e3ef99cde5b6e313922a5ccce7f38e17f790ac/twubric.json');
      users.value = response.data;
    };

    const toggleSort = (field) => {
      if (sortBy.value === field) {
        users.value.reverse();
      } else {
        sortBy.value = field;
      }
    };

    const filteredAndSortedData = computed(() => {
      let result = [...users.value];
      const start = startDate.value ? new Date(startDate.value).getTime() / 1000 : null;
      const end = endDate.value ? new Date(endDate.value).getTime() / 1000 : null;

      if (start || end) {
        result = result.filter(user => {
          const joinedDate = user.join_date;
          return (!start || joinedDate >= start) && (!end || joinedDate <= end);
        });
      }

      if (sortBy.value) {
        result.sort((a, b) => a.twubric[sortBy.value] - b.twubric[sortBy.value]);
      }

      return result;
    });

    const removeUser = (id) => {
      users.value = users.value.filter(user => user.uid !== id);
    };

    const formatDate = (timestamp) => {
      if (!timestamp) return 'N/A';
      const date = new Date(timestamp * 1000);
      return date.toLocaleDateString('en-US', { year: 'numeric', month: 'short', day: 'numeric' });
    };

    onMounted(fetchData);

    return {
      sortBy,
      startDate,
      endDate,
      filteredAndSortedData,
      toggleSort,
      removeUser,
      formatDate
    };
  }
};
</script>

<style scoped>
.container {
  max-width: 1200px;
}
</style>
