<template>
  <div class="global-container">
    <table>
      <thead>
        <tr>
          <th>Name</th>
          <th>Email</th>
          <th>Address</th>
          <th>Phone</th>
          <th>Website</th>
          <th>Company</th>
          <th>Details</th>
          <th>Action</th>
        </tr>
      </thead>
      <tbody>
        <tr v-for="(user, index) in users" :key="user.id">
          <td>{{ user.name }}</td>
          <td>{{ user.email }}</td>
          <td>
            {{ user.address.street }}, {{ user.address.suite }}, {{ user.address.city }},
            {{ user.address.zipcode }}
          </td>
          <td>{{ user.phone }}</td>
          <td>{{ user.website }}</td>
          <td>{{ user.company.name }}</td>
          <td><button @click="showModal(user)">View Details</button></td>
          <td><button @click="removeUser(index)">X</button></td>
        </tr>
      </tbody>
    </table>
    <div v-if="selectedUser" class="modal">
      <div class="modal-content">
        <h2>{{ selectedUser.name }}</h2>
        <p>Email: {{ selectedUser.email }}</p>
        <p>
          Address: {{ selectedUser.address.street }}, {{ selectedUser.address.suite }},
          {{ selectedUser.address.city }}, {{ selectedUser.address.zipcode }}
        </p>
        <p>Phone: {{ selectedUser.phone }}</p>
        <p>Website: {{ selectedUser.website }}</p>
        <p>Company: {{ selectedUser.company.name }}</p>
        <p>Latitude: {{ selectedUser.address.geo.lat }}</p>
        <p>Longitude: {{ selectedUser.address.geo.lng }}</p>
        <button @click="selectedUser = null">Close</button>
      </div>
    </div>
  </div>
</template>

<script setup lang="ts">
import { ref, onMounted } from 'vue'
import axios from 'axios'

const users = ref([])
const selectedUser = ref(null)

onMounted(async () => {
  const response = await axios.get('https://jsonplaceholder.typicode.com/users')
  users.value = response.data
})

function showModal(user) {
  selectedUser.value = user
}

function removeUser(index) {
  users.value.splice(index, 1)
}
</script>

<style scoped>
.global-container {
  padding: 1rem;
}

table {
  width: 100%;
  border-collapse: collapse;
}

th,
td {
  padding: 8px;
  border: 1px solid #ddd;
  background-color: #f5f5f5;
}

th {
  text-align: left;
  background-color: #3498db;
  color: white;
}

tr:nth-child(even) {
  background-color: #f2f2f2;
}

tr:hover {
  background-color: #ccc;
}

.modal {
  display: block;
  position: fixed;
  z-index: 1;
  left: 0;
  top: 0;
  width: 100%;
  height: 100%;
  overflow: auto;
  background-color: rgba(0, 0, 0, 0.4);
}

.modal-content {
  background-color: #fefefe;
  margin: 15% auto;
  padding: 20px;
  border: 1px solid #888;
  width: 400px;
}

.modal-content button {
  background-color: #3498db;
  color: white;
  padding: 14px 20px;
  margin: 8px 0;
  border: none;
  cursor: pointer;
}

.modal-content button:hover {
  background-color: #2980b9;
}
</style>
