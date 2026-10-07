<!-- eslint-disable vue/multi-word-component-names -->
<script setup lang="ts">
import { ref } from 'vue'
import userData from '../data/user.json'
import type { User } from '../types/User'

const user: User = userData

const showDetails = ref(false)

const ageClass = {
  minor: user.dob.age < 18,
  young: user.dob.age >= 18 && user.dob.age <= 30,
  adult: user.dob.age >= 31 && user.dob.age <= 50,
  senior: user.dob.age > 50,
}
</script>

<template>
  <div
    class="user-card"
    :class="ageClass"
  >
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

        <p v-if="user.dob.age > 18">
          <span>Вік:</span>
          {{ user.dob.age }}
        </p>
      </div>

      <div class="hobbies">
        <h3>Хобі</h3>

        <ul>
          <li
            v-for="hobby in user.hobbies"
            :key="hobby"
          >
            {{ hobby }}
          </li>
        </ul>
      </div>

      <button
        class="details-button"
        @click="showDetails = !showDetails"
      >
        {{ showDetails ? 'Сховати деталі' : 'Показати деталі' }}
      </button>

      <p
        v-show="showDetails"
        class="details"
      >
        {{ user.details }}
      </p>
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

.user-card.minor {
  border-top: 5px solid #60a5fa;
}

.user-card.young {
  border-top: 5px solid #34d399;
}

.user-card.adult {
  border-top: 5px solid #f59e0b;
}

.user-card.senior {
  border-top: 5px solid #ef4444;
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

.hobbies h3 {
  margin: 20px 0 8px;
  color: #1f2937;
}

.hobbies ul {
  margin: 0;
  padding-left: 20px;
}

.hobbies li {
  margin-bottom: 4px;
  color: #4b5563;
}

.details-button {
  margin-top: 20px;
  padding: 10px 16px;
  border: none;
  border-radius: 8px;
  background: #2563eb;
  color: white;
  cursor: pointer;
  font-size: 14px;
}

.details-button:hover {
  background: #1d4ed8;
}

.details {
  margin-top: 12px;
  padding: 12px;
  border-radius: 8px;
  background: #f3f4f6;
  color: #4b5563;
}
</style>