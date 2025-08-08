<script setup>
import ArtworkRating from './components/ArtworkRating.vue'

// Define categories for artwork rating
const artworkCategories = [
  { key: 'fitsToWall', label: 'Fits to my Wall' },
  { key: 'style', label: 'That\'s my Style' },
  { key: 'artist', label: 'Enjoy this Artist' }
]

</script>

<template>
  <div class="artwork-rating-page">
    <main class="page-main">
      <ArtworkRating
        artwork-id="artwork-001"
        artwork-title="Sample Artwork"
        :categories="artworkCategories"
      />
    </main>
  </div>
</template>
