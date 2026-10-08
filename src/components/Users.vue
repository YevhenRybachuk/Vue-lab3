<!-- eslint-disable vue/multi-word-component-names -->
<script setup lang="ts">
import { computed, ref } from 'vue'
import userData from '../data/user.json'
import type { User } from '../types/User'

const users: User[] = userData

const genderFilter = ref('all')
const ageFilter = ref('all')
const sortBy = ref('name-asc')

const openedDetails = ref<string | null>(null)

const filteredUsers = computed(() => {
  let result = [...users]

  if (genderFilter.value !== 'all') {
    result = result.filter(
      user => user.gender === genderFilter.value
    )
  }

  if (ageFilter.value === '18+') {
    result = result.filter(user => user.dob.age >= 18)
  }

  result.sort((a, b) => {
    switch (sortBy.value) {
      case 'name-asc':
        return a.name.first.localeCompare(b.name.first)

      case 'name-desc':
        return b.name.first.localeCompare(a.name.first)

      case 'age-asc':
        return a.dob.age - b.dob.age

      case 'age-desc':
        return b.dob.age - a.dob.age

      default:
        return 0
    }
  })

  return result
})

function toggleDetails(email: string) {
  if (openedDetails.value === email) {
    openedDetails.value = null
  } else {
    openedDetails.value = email
  }
}

function clearFilters() {
  genderFilter.value = 'all'
  ageFilter.value = 'all'
  sortBy.value = 'name-asc'
  openedDetails.value = null
}
</script>

<template>
  <div class="users-container">

    <div class="toolbar">
      <div class="filter-group">
        <span class="filter-label">Стать:</span>

        <button
          :class="{ active: genderFilter === 'all' }"
          @click="genderFilter = 'all'"
        >
          Всі
        </button>

        <button
          :class="{ active: genderFilter === 'male' }"
          @click="genderFilter = 'male'"
        >
          Чоловіки
        </button>

        <button
          :class="{ active: genderFilter === 'female' }"
          @click="genderFilter = 'female'"
        >
          Жінки
        </button>
      </div>

      <div class="filter-group">
        <span class="filter-label">Вік:</span>

        <button
          :class="{ active: ageFilter === 'all' }"
          @click="ageFilter = 'all'"
        >
          Всі
        </button>

        <button
          :class="{ active: ageFilter === '18+' }"
          @click="ageFilter = '18+'"
        >
          18+
        </button>
      </div>

      <div class="filter-group">
        <span class="filter-label">Сортування:</span>

        <button
          :class="{ active: sortBy === 'name-asc' }"
          @click="sortBy = 'name-asc'"
        >
          Ім'я ↑
        </button>

        <button
          :class="{ active: sortBy === 'name-desc' }"
          @click="sortBy = 'name-desc'"
        >
          Ім'я ↓
        </button>

        <button
          :class="{ active: sortBy === 'age-asc' }"
          @click="sortBy = 'age-asc'"
        >
          Вік ↑
        </button>

        <button
          :class="{ active: sortBy === 'age-desc' }"
          @click="sortBy = 'age-desc'"
        >
          Вік ↓
        </button>
      </div>

      <button
        class="clear-button"
        @click="clearFilters"
      >
        Очистити все
      </button>
    </div>

    <div
      v-if="filteredUsers.length > 0"
      class="users-list"
    >
      <article
        v-for="user in filteredUsers"
        :key="user.email"
        class="user-card"
        :class="{
          minor: user.dob.age < 18,
          young: user.dob.age >= 18 && user.dob.age < 30,
          adult: user.dob.age >= 30 && user.dob.age < 60,
          senior: user.dob.age >= 60
        }"
      >
        <img
          :src="user.picture"
          :alt="`${user.name.first} ${user.name.last}`"
          class="user-photo"
        >

        <div class="user-info">
          <h2>
            {{ user.name.first }} {{ user.name.last }}
          </h2>

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
            @click="toggleDetails(user.email)"
          >
            {{
              openedDetails === user.email
                ? 'Приховати деталі'
                : 'Показати деталі'
            }}
          </button>

          <div
            v-show="openedDetails === user.email"
            class="details"
          >
            <strong>Додаткова інформація:</strong>
            <p>{{ user.details }}</p>
          </div>
        </div>
      </article>
    </div>

    <p
      v-else
      class="empty-message"
    >
      Користувачів не знайдено
    </p>

  </div>
</template>

<style scoped>
.users-container {
  width: 100%;
}

/* Панель керування */
.toolbar {
  width: 100%;
  display: flex;
  flex-wrap: wrap;
  align-items: center;
  gap: 12px;
  margin-bottom: 30px;
  padding: 20px;
  background: white;
  border-radius: 16px;
  box-shadow: 0 4px 15px rgb(0 0 0 / 8%);
}

.filter-group {
  display: flex;
  align-items: center;
  flex-wrap: wrap;
  gap: 8px;
}

.filter-label {
  font-weight: 600;
  color: #1f2937;
}

.toolbar button {
  padding: 9px 14px;
  border: 1px solid #d1d5db;
  border-radius: 8px;
  background: white;
  color: #374151;
  cursor: pointer;
  font-size: 14px;
  transition: 0.2s;
}

.toolbar button:hover {
  background: #f3f4f6;
}

.toolbar button.active {
  background: #2563eb;
  color: white;
  border-color: #2563eb;
}

.clear-button {
  margin-left: auto;
  background: #ef4444 !important;
  color: white !important;
  border-color: #ef4444 !important;
}

.clear-button:hover {
  background: #dc2626 !important;
}

/* Сітка карток */
.users-list {
  width: 100%;
  display: grid;
  grid-template-columns: repeat(2, minmax(0, 1fr));
  gap: 24px;
}

/* Картка користувача */
.user-card {
  width: 100%;
  min-width: 0;
  display: flex;
  flex-direction: row;
  align-items: flex-start;
  gap: 25px;
  padding: 25px;
  background: white;
  border-radius: 16px;
  box-shadow: 0 8px 25px rgb(0 0 0 / 12%);
  border: 1px solid #e5e7eb;
  transition: transform 0.2s, box-shadow 0.2s;
}

.user-card:hover {
  transform: translateY(-3px);
  box-shadow: 0 12px 30px rgb(0 0 0 / 16%);
}

/* Кольорова смужка залежно від віку */
.user-card.minor {
  border-left: 5px solid #f59e0b;
}

.user-card.young {
  border-left: 5px solid #22c55e;
}

.user-card.adult {
  border-left: 5px solid #3b82f6;
}

.user-card.senior {
  border-left: 5px solid #8b5cf6;
}

/* Фото */
.user-photo {
  width: 130px;
  height: 130px;
  flex-shrink: 0;
  border-radius: 50%;
  object-fit: cover;
  border: 4px solid #f1f5f9;
}

/* Інформація */
.user-info {
  flex: 1;
  min-width: 0;
}

.user-info h2 {
  margin: 0 0 18px;
  color: #1f2937;
  font-size: 24px;
}

/* Основні дані */
.user-details {
  display: flex;
  flex-direction: column;
  gap: 7px;
}

.user-details p {
  margin: 0;
  color: #4b5563;
  font-size: 14px;
  line-height: 1.4;
  overflow-wrap: anywhere;
}

.user-details span {
  display: inline-block;
  min-width: 125px;
  font-weight: 600;
  color: #1f2937;
}

/* Хобі */
.hobbies h3 {
  margin: 18px 0 8px;
  color: #1f2937;
  font-size: 17px;
}

.hobbies ul {
  margin: 0;
  padding-left: 20px;
}

.hobbies li {
  margin-bottom: 3px;
  color: #4b5563;
  font-size: 14px;
}

/* Кнопка деталей */
.details-button {
  width: 100%;
  margin-top: 18px;
  padding: 10px 16px;
  border: none;
  border-radius: 8px;
  background: #2563eb;
  color: white;
  font-size: 14px;
  font-weight: 600;
  cursor: pointer;
  transition: 0.2s;
}

.details-button:hover {
  background: #1d4ed8;
}

/* Деталі */
.details {
  width: 100%;
  margin-top: 12px;
  padding: 14px;
  background: #f8fafc;
  border-radius: 10px;
  color: #4b5563;
  font-size: 14px;
}

.details p {
  margin: 8px 0 0;
  line-height: 1.5;
}

/* Порожній список */
.empty-message {
  margin: 50px auto;
  text-align: center;
  color: #6b7280;
  font-size: 20px;
}

/* Планшет */
@media (max-width: 1100px) {
  .users-list {
    grid-template-columns: 1fr;
  }
}

/* Телефон */
@media (max-width: 650px) {
  main {
    padding: 15px;
  }

  .user-card {
    flex-direction: column;
    align-items: center;
  }

  .user-info {
    width: 100%;
  }

  .user-info h2 {
    text-align: center;
  }

  .toolbar {
    flex-direction: column;
    align-items: stretch;
  }

  .filter-group {
    justify-content: center;
  }

  .clear-button {
    margin-left: 0;
  }
}
</style>