<script setup>
import ArtworkRating from '../components/ArtworkRating.vue'

// Define categories for artwork rating
const artworkCategories = [
  { key: 'fitsToWall', label: 'Fits to my Wall' },
  { key: 'style', label: 'That\'s my Style' },
  { key: 'artist', label: 'Enjoy this Artist' }
]
</script>

<template>
  <div class="artwork-rating-page">
    <header>
      <h1>Lumas Art Rating System</h1>
    </header>

    <main class="page-main">
      <ArtworkRating
        :categories="artworkCategories"
      />
    </main>
  </div>
</template>

<style scoped>
.artwork-rating-page {
  width: 100%;
  max-width: 600px;
  margin: 0 auto;
  padding: 2rem;
  display: flex;
  flex-direction: column;
  align-items: center;
  min-height: 100vh;
  box-sizing: border-box;
}

.page-title {
  font-size: 2rem;
  font-weight: 700;
  color: #2c3e50;
  text-align: center;
  margin-bottom: 2rem;
  line-height: 1.2;
}

.page-description {
  font-size: 1.1rem;
  color: #666;
  text-align: center;
  margin-bottom: 2rem;
  line-height: 1.5;
  max-width: 480px;
}

/* Mobile styles */
@media (max-width: 768px) {
  .artwork-rating-page {
    width: 100%;
    max-width: none;
    padding: 1.5rem 1rem;
    min-height: calc(100vh - 2rem);
  }
  
  .page-title {
    font-size: 1.75rem;
    margin-bottom: 1.5rem;
  }
  
  .page-description {
    font-size: 1rem;
    margin-bottom: 1.5rem;
    max-width: 100%;
    padding: 0 0.5rem;
  }
}
</style>