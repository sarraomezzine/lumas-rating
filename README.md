# Lumas Art Rating System

A comprehensive, reusable star rating component built with Vue 3 and modern best practices. This system allows users to rate artworks across multiple categories with interactive star ratings.

## Features

### Core Functionality
- **Multi-Category Rating**: Fully configurable rating categories via props - customize for any use case
- **Interactive Star Rating**: Click to set ratings, hover for visual feedback
- **Real-time Updates**: Live display of ratings with numerical feedback
- **Reusable Components**: Modular, composable architecture

### Technical Features
- **Page-based Architecture**: Clean separation of concerns with dedicated page components
- **Props-driven Components**: Configurable categories for maximum reusability
- **Mobile-first Design**: Responsive layout with touch-friendly interactions

## Quick Start

### Prerequisites
- Node.js 20.19.0+ or 22.12.0+
- npm or yarn

### Installation & Setup

1. **Clone and install dependencies**:
```bash
git clone git@github.com:sarraomezzine/lumas-rating.git
npm install
```

2. **Start development server**:
```bash
npm run dev
```

3. **Build for production**:
```bash
npm run build
```

## Project Structure

```
src/
├── components/
│   ├── StarRating.vue          # Reusable star rating component
│   └── ArtworkRating.vue       # Main rating interface
├── pages/
│   └── ArtworkRatingPage.vue   # Page-level component with business logic
responsive design
├── App.vue                    # Main application shell
└── main.js                    # Application entry point
```