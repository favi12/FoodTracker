<script setup>
import { onMounted, ref } from 'vue'

const foods = ref([])
const error = ref('')

const apiUrl = import.meta.env.VITE_API_URL || 'http://localhost:8080'

function foodName(food) {
  return typeof food === 'string' ? food : food.name
}

onMounted(async () => {
  try {
    const response = await fetch(`${apiUrl}/api/foods`)

    if (!response.ok) {
      throw new Error('Fehler beim Laden')
    }

    foods.value = await response.json()
  } catch {
    error.value = 'Die Liste konnte nicht geladen werden.'
  }
})

// Platzhalter fuer M4:
// async function addFood() {
//   const newFood = {
//     name: 'Reis',
//     calories: 130,
//     protein: 2.7,
//     carbohydrates: 28,
//     fat: 0.3,
//   }
//
//   const response = await fetch(`${apiUrl}/api/foods`, {
//     method: 'POST',
//     headers: {
//       'Content-Type': 'application/json',
//     },
//     body: JSON.stringify(newFood),
//   })
//
//   const savedFood = await response.json()
//   foods.value.push(savedFood)
// }
</script>

<template>
  <section class="food-list">
    <h2>Meine Mahlzeiten</h2>

    <p v-if="error" class="error">{{ error }}</p>

    <ul v-else class="foods">
      <li v-for="food in foods" :key="food.id || foodName(food)" class="food-card">
        <div>
          <strong>{{ foodName(food) }}</strong>
          <p v-if="food.calories">{{ food.calories }} kcal</p>
        </div>

        <div v-if="food.protein !== undefined" class="nutrition">
          <span>Protein: {{ food.protein }} g</span>
          <span>Kohlenhydrate: {{ food.carbohydrates }} g</span>
          <span>Fett: {{ food.fat }} g</span>
        </div>
      </li>
    </ul>
  </section>
</template>

<style scoped>
.food-list {
  background: white;
  padding: 24px;
  border-radius: 8px;
  box-shadow: 0 2px 12px rgba(0, 0, 0, 0.08);
}

h2 {
  margin-top: 0;
}

.foods {
  list-style: none;
  padding: 0;
  margin: 0;
}

.food-card {
  display: flex;
  justify-content: space-between;
  gap: 24px;
  padding: 16px 0;
  border-bottom: 1px solid #e5e7eb;
}

.food-card:last-child {
  border-bottom: 0;
}

.food-card p {
  margin: 6px 0 0;
  color: #52616b;
}

.nutrition {
  display: flex;
  flex-wrap: wrap;
  gap: 8px;
  justify-content: flex-end;
  color: #52616b;
  font-size: 14px;
}

.error {
  color: #b91c1c;
}

@media (max-width: 640px) {
  .food-card {
    display: block;
  }

  .nutrition {
    justify-content: flex-start;
    margin-top: 12px;
  }
}
</style>
