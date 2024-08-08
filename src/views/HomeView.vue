<template>
  <div class="passenger-list-container">
    <h1 class="title">Passenger Name</h1>

    <div v-if="error" class="error">{{ error }}</div>
     <ul v-else class="passenger-list">
      <li v-for="passenger in passengers" :key="passenger._id" class="passenger-item">
        <router-link :to="{ name: 'PassengerDetail', params: { id: passenger._id } }" class="passenger-link">
          {{ passenger.name }}
        </router-link>
      </li>
    </ul>
  </div>
</template>

<style scoped>
.passenger-list-container {
  max-width: 1200px; 
  margin: 0 auto;
  padding: 1rem; 
}

.title {
  margin-bottom: 1rem;
}

.passenger-link {
  display: block;
  grid-template-columns: repeat(2, 1fr);
  padding: 1rem;
  background-color: #fff;
  border-radius: 8px;
  box-shadow: 0 2px 6px rgba(0, 0, 0, 0.1);
  text-decoration: none;
  color: #333;
  transition: background-color 0.3s, color 0.3s;
}

.passenger-link:hover {
  background-color: #06ee76; 
  color: #3107d7; 
}

.passenger-list {
  display: grid;
  grid-template-columns: repeat(2, 1fr);
  list-style-type: none; 
  padding: 0; 
  margin: 0; 
}

.error {
  color: rgb(89, 72, 168);
  font-weight: bold;
}
</style>

<script lang="ts">
import { defineComponent, ref, onMounted } from 'vue';
import { getPassengers } from '@/services/apiService';

export default defineComponent({
  name: 'HomeView',
  setup() {
    const passengers = ref<any[]>([]);
    const error = ref<string | null>(null);

    onMounted(async () => {
      try {
        const response = await getPassengers();
        passengers.value = response.data.data; 
      } catch (err) {
        error.value = (err as Error).message;
        console.error("There was an error!", err);
      }
    });

    return { passengers, error };
  },
});
</script>

<style scoped>
.error {
  color: rgb(89, 72, 168);
  font-weight: bold;
}
</style>
