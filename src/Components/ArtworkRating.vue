<script setup>
import { reactive } from 'vue'
import StarRating from './StarRating.vue'

const props = defineProps({
  categories: {
    type: Array,
    default: () => [
      { key: 'rating1', label: 'Rating 1' },
      { key: 'rating2', label: 'Rating 2' },
      { key: 'rating3', label: 'Rating 3' }
    ],
    validator: (categories) => {
      return categories.every(cat => 
        cat && 
        typeof cat.key === 'string' && 
        typeof cat.label === 'string'
      )
    }
  }
})

// Create reactive ratings object dynamically based on categories prop
const ratings = reactive(
  props.categories.reduce((acc, category) => {
    acc[category.key] = 0
    return acc
  }, {})
)
</script>

<template>
  <div class="artwork-rating">
    <h2 class="rating-title">Rate this Artwork</h2>
    
    <div class="rating-categories">
      <div
        v-for="category in props.categories"
        :key="category.key"
        class="rating-category"
      >
        <label class="category-label" :for="`rating-${category.key}`">
          {{ category.label }}
        </label>
        <StarRating
          :id="`rating-${category.key}`"
          v-model="ratings[category.key]"
        />
      </div>
    </div>
  </div>
</template>

<style scoped>
.artwork-rating {
  width: 100%;
  max-width: 500px;
  margin: 0 auto;
  padding: 2rem;
  background: #fff;
  border-radius: 12px;
  box-shadow: 0 4px 6px rgba(0, 0, 0, 0.1);
}

.rating-title {
  font-size: 1.5rem;
  font-weight: 700;
  color: #333;
  margin-bottom: 1.5rem;
  text-align: center;
  line-height: 1.3;
}

.rating-categories {
  display: flex;
  flex-direction: column;
  gap: 1.5rem;
  margin-bottom: 2rem;
}

.rating-category {
  display: flex;
  flex-direction: column;
  gap: 0.75rem;
  padding: 1rem;
  background: #f8f9fa;
  border-radius: 8px;
  border: 1px solid #e9ecef;
}

.category-label {
  font-size: 1rem;
  font-weight: 600;
  color: #555;
  text-align: center;
}
</style>
