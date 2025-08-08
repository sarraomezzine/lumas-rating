<script setup>
import { ref, computed, reactive } from 'vue'
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

const submissionMessage = ref('')
const submissionMessageType = ref('success')

// Computed properties
const hasAnyRating = computed(() => {
  return Object.values(ratings).some(rating => rating > 0)
})

const averageRating = computed(() => {
  const validRatings = Object.values(ratings).filter(rating => rating > 0)
  if (validRatings.length === 0) return 0
  return (validRatings.reduce((sum, rating) => sum + rating, 0) / validRatings.length).toFixed(1)
})


const submitRatings = () => {
  if (!hasAnyRating.value) {
    submissionMessage.value = 'Please provide at least one rating before submitting.'
    submissionMessageType.value = 'error'
    return
  }

  const submissionData = {
    ratings: { ...ratings },
    averageRating: averageRating.value,
  }

  // Log to console as requested
  console.log('Rating Submission:', submissionData)

  // Show success message
  submissionMessage.value = `Ratings submitted successfully!\nAverage: ${averageRating.value}/5`
  submissionMessageType.value = 'success'

  // Clear message after 3 seconds
  setTimeout(() => {
    submissionMessage.value = ''
  }, 3000)
}

const resetRatings = () => {
  Object.keys(ratings).forEach(key => {
    ratings[key] = 0
  })
  submissionMessage.value = 'All ratings have been reset.'
  submissionMessageType.value = 'info'
  
  // Clear message after 2 seconds
  setTimeout(() => {
    submissionMessage.value = ''
  }, 2000)
}
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

    <div class="rating-actions">
      <button
        type="button"
        class="submit-button"
        :disabled="!hasAnyRating"
        @click="submitRatings"
      >
        Submit Ratings
      </button>
      <button
        type="button"
        class="reset-button"
        :disabled="!hasAnyRating"
        @click="resetRatings"
      >
        Reset All
      </button>
    </div>

    <div class="submission-message-container">
      <div v-show="submissionMessage" class="submission-message" :class="submissionMessageType">
        {{ submissionMessage }}
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

.rating-actions {
  display: flex;
  gap: 1rem;
  justify-content: center;
  margin-bottom: 1rem;
  flex-wrap: wrap;
}

.submit-button,
.reset-button {
  padding: 0.875rem 1.5rem;
  border: none;
  border-radius: 8px;
  font-size: 1rem;
  font-weight: 600;
  cursor: pointer;
  transition: all 0.2s ease;
  min-width: 120px;
  touch-action: manipulation;
}

.submit-button {
  background: #007acc;
  color: white;
}

.submit-button:hover:not(:disabled) {
  background: #005fa3;
  transform: translateY(-1px);
}

.reset-button {
  background: #6c757d;
  color: white;
}

.reset-button:hover:not(:disabled) {
  background: #545b62;
  transform: translateY(-1px);
}

.submit-button:disabled,
.reset-button:disabled {
  background: #e9ecef;
  color: #6c757d;
  cursor: not-allowed;
  transform: none;
}

.submission-message-container {
  min-height: 3rem;
  margin-top: 1rem;
  display: flex;
  align-items: center;
  justify-content: center;
}

.submission-message {
  padding: 0.75rem;
  border-radius: 6px;
  text-align: center;
  font-weight: 500;
  width: 100%;
  transition: opacity 0.3s ease;
  word-wrap: break-word;
  overflow-wrap: break-word;
  hyphens: auto;
  white-space: pre-line;
}

.submission-message.success {
  background: #d1edff;
  color: #0c5460;
  border: 1px solid #b8daff;
}

.submission-message.error {
  background: #f8d7da;
  color: #721c24;
  border: 1px solid #f5c6cb;
}

.submission-message.info {
  background: #d4edda;
  color: #155724;
  border: 1px solid #c3e6cb;
}

/* Mobile styles */
@media (max-width: 768px) {
  .artwork-rating {
    width: calc(100vw - 2rem);
    max-width: none;
    padding: 1.5rem;
    margin: 1rem;
    border-radius: 8px;
  }
  
  .rating-title {
    font-size: 1.375rem;
    margin-bottom: 1.25rem;
  }
  
  .rating-categories {
    gap: 1.25rem;
    margin-bottom: 1.5rem;
  }
  
  .rating-category {
    gap: 1rem;
    padding: 1rem;
  }
  
  .category-label {
    font-size: 0.95rem;
    line-height: 1.4;
  }
  
  .rating-actions {
    flex-direction: column;
    align-items: center;
    gap: 0.75rem;
  }
  
  .submit-button,
  .reset-button {
    width: 100%;
    max-width: 280px;
    padding: 1rem 1.5rem;
    font-size: 1.05rem;
  }
  
  .submission-message {
    font-size: 0.9rem;
    padding: 1rem;
  }
}
</style>
