<!-- eslint-disable vue/multi-word-component-names -->
<script setup lang="ts">
import userData from '../data/user.json'
import type { User } from '../types/User'

const user: User = userData
</script>

<template>
  <div class="user-card">
    <img
      :src="user.picture"
      :alt="`${user.name.first} ${user.name.last}`"
      class="user-photo"
    >

    <div class="user-info">
      <h2>{{ user.name.first }} {{ user.name.last }}</h2>

      <div class="user-details">
        <p>
          <span>Стать:</span>
          {{ user.gender === 'male' ? 'Чоловік' : 'Жінка' }}
        </p>

        <p>
          <span>Місто:</span>
          {{ user.location.city }}
        </p>

        <p>
          <span>Країна:</span>
          {{ user.location.country }}
        </p>

        <p>
          <span>Email:</span>
          {{ user.email }}
        </p>

        <p>
          <span>Телефон:</span>
          {{ user.phone }}
        </p>

        <p>
          <span>Дата народження:</span>
          {{ user.dob.date }}
        </p>

        <p>
          <span>Вік:</span>
          {{ user.dob.age }}
        </p>
      </div>
    </div>
  </div>
</template>

<style scoped>
.user-card {
  display: flex;
  align-items: center;
  gap: 30px;
  width: 600px;
  margin: 40px auto;
  padding: 30px;
  background: white;
  border-radius: 16px;
  box-shadow: 0 8px 25px rgb(0 0 0 / 12%);
  border: 1px solid #e5e7eb;
}

.user-photo {
  width: 160px;
  height: 160px;
  flex-shrink: 0;
  border-radius: 50%;
  object-fit: cover;
  border: 4px solid #f1f5f9;
}

.user-info {
  flex: 1;
}

.user-info h2 {
  margin: 0 0 20px;
  color: #1f2937;
  font-size: 28px;
}

.user-details {
  display: flex;
  flex-direction: column;
  gap: 8px;
}

.user-details p {
  margin: 0;
  color: #4b5563;
  font-size: 15px;
  line-height: 1.5;
}

.user-details span {
  display: inline-block;
  min-width: 150px;
  font-weight: 600;
  color: #1f2937;
}
</style>