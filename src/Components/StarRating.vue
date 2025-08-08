<script setup>
import { ref, computed, watch } from 'vue'

const props = defineProps({
  modelValue: {
    type: Number,
    default: 0,
    validator: (value) => value >= 0 && value <= 10
  },
  maxStars: {
    type: Number,
    default: 5,
    validator: (value) => value > 0 && value <= 10
  },
  showRatingText: {
    type: Boolean,
    default: true
  },
})

const emit = defineEmits(['update:modelValue', 'change'])

const currentRating = ref(props.modelValue)
const hoverRating = ref(0)

const displayRating = computed(() => {
  return hoverRating.value > 0 ? hoverRating.value : currentRating.value
})

const setRating = (rating) => {  
  currentRating.value = rating
  emit('update:modelValue', rating)
  emit('change', rating)
}

const setHoverRating = (rating) => {
  hoverRating.value = rating
}

const clearHoverRating = () => {
  hoverRating.value = 0
}

// Watch for external changes to modelValue
watch(() => props.modelValue, (newValue) => {
  currentRating.value = newValue
})
</script>


<template>
  <div class="star-rating">
    <div class="stars-container">
      <button
        v-for="star in maxStars"
        :key="star"
        type="button"
        class="star-button"
        :class="{
          'star-filled': star <= currentRating,
          'star-hover': star <= hoverRating && hoverRating > 0
        }"
        @click="setRating(star)"
        @mouseenter="setHoverRating(star)"
        @mouseleave="clearHoverRating"
        :aria-label="`Rate ${star} out of ${maxStars} stars`"
      >
        ⭐
      </button>
    </div>
    <div class="rating-display" v-if="showRatingText">
      {{ displayRating }}/{{ maxStars }}
    </div>
  </div>
</template>


<style scoped>
.star-rating {
  display: flex;
  align-items: center;
  gap: 0.5rem;
  flex-wrap: wrap;
}

.stars-container {
  display: flex;
  gap: 0.25rem;
  flex-wrap: wrap;
}

.star-button {
  background: none;
  border: none;
  font-size: 1.5rem;
  cursor: pointer;
  padding: 0.25rem;
  border-radius: 0.25rem;
  transition: all 0.2s ease;
  opacity: 0.3;
  transform: scale(1);
  min-width: 44px; 
  min-height: 44px; 
  display: flex;
  align-items: center;
  justify-content: center;
}

.star-button:hover {
  transform: scale(1.2);
}

.star-button.star-filled,
.star-button.star-hover {
  opacity: 1;
}

.rating-display {
  font-size: 0.9rem;
  font-weight: 600;
  color: #666;
  min-width: 2rem;
  white-space: nowrap;
}

/* Mobile devices */
@media (max-width: 768px) {
  .star-rating {
    gap: 0.375rem;
    justify-content: center;
  }
  
  .stars-container {
    gap: 0.125rem;
    justify-content: center;
  }
  
  .star-button {
    font-size: 1.375rem;
    padding: 0.125rem;
    min-width: 40px;
    min-height: 40px;
  }
  
  .rating-display {
    font-size: 0.875rem;
    text-align: center;
    width: 100%;
  }
}

</style>