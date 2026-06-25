<template>
  <div id="app" class="container">
    <header class="app-header">
      <div class="brand">
        <div>
          <h1>Luxury & Culture</h1>
          <p class="tagline">Short-term high-end rentals in Cape Town</p>
        </div>
      </div>
      <div class="counter-badge">
        Active Listings: <strong>{{ totalActiveListings }}</strong>
      </div>
    </header>

    <FilterControls v-model:searchQuery="searchQuery" v-model:sortBy="sortBy" />

    <main class="properties-grid">
      <PropertyCard
        v-for="property in filteredAndSortedProperties"
        :key="property.id"
        :property="property"
        :is-bookmarked="bookmarks.includes(property.id)"
        @toggle-bookmark="handleBookmarkToggle"
      />
    </main>

    <div v-if="filteredAndSortedProperties.length === 0" class="no-results">
      <p>
        No properties match your search criteria. Try using different search terms.
      </p>
    </div>
  </div>
</template>

<script>
import { propertiesData } from "./data/properties.js";
import FilterControls from "./components/FilterControls.vue";
import PropertyCard from "./components/PropertyCard.vue";

export default {
  name: "App",
  components: {
    FilterControls,
    PropertyCard,
  },
  data() {
    return {
      properties: propertiesData,
      searchQuery: "",
      sortBy: "default",
      bookmarks: JSON.parse(localStorage.getItem("bookmarked_properties")) || [],
    };
  },
  computed: {
    totalActiveListings() {
      return this.properties.filter((p) => p.isAvailable).length;
    },
    filteredAndSortedProperties() {
      let result = [...this.properties];

      if (this.searchQuery) {
        const query = this.searchQuery.toLowerCase().trim();
        result = result.filter(
          (p) =>
            p.title.toLowerCase().includes(query) ||
            p.location.toLowerCase().includes(query),
        );
      }

      if (this.sortBy === "low-high") {
        result.sort((a, b) => a.price - b.price);
      } else if (this.sortBy === "high-low") {
        result.sort((a, b) => b.price - a.price);
      }

      return result;
    },
  },
  methods: {
    handleBookmarkToggle(propertyId) {
      const index = this.bookmarks.indexOf(propertyId);
      if (index > -1) {
        this.bookmarks.splice(index, 1);
      } else {
        this.bookmarks.push(propertyId);
      }
      localStorage.setItem(
        "bookmarked_properties",
        JSON.stringify(this.bookmarks),
      );
    },
  },
};
</script>

<style>

* {
  box-sizing: border-box;
  margin: 0;
  padding: 0;
  font-family: "Cinzel", "Didot", "Playfair Display", "Segoe UI", serif; 
}

body {
  background-color: #0b0c10; 
  background-image: 
    radial-gradient(circle at 20% 30%, rgba(204, 164, 59, 0.03) 0%, transparent 40%),
    radial-gradient(circle at 80% 70%, rgba(255, 255, 255, 0.02) 0%, transparent 50%);
  color: #f5f5f7; 
  padding: 40px 0;
  min-height: 100vh;
}

.container {
  max-width: 1200px;
  margin: 0 auto;
  padding: 0 30px;
}

.app-header {
  display: flex;
  justify-content: space-between;
  align-items: center;
  padding-bottom: 30px;
  border-bottom: 1px solid rgba(204, 164, 59, 0.15); 
  margin-bottom: 40px;
}

.brand {
  display: flex;
  align-items: center;
  gap: 12px;
}

.app-header h1 {
  font-size: 2.2rem;
  font-weight: 300;
  letter-spacing: 2px;
  text-transform: uppercase;
  background: linear-gradient(135deg, #ffffff 0%, #dfba73 50%, #cca43b 100%);
  -webkit-background-clip: text;
  -webkit-text-fill-color: transparent;
}

.tagline {
  font-size: 0.85rem;
  color: #a0a5b5;
  letter-spacing: 1px;
  margin-top: 4px;
  text-transform: uppercase;
}

.counter-badge {
  background-color: rgba(204, 164, 59, 0.08);
  color: #dfba73;
  padding: 8px 18px;
  border-radius: 0px; 
  font-size: 0.8rem;
  text-transform: uppercase;
  letter-spacing: 1px;
  border: 1px solid rgba(204, 164, 59, 0.3);
}

.properties-grid {
  display: grid;
  grid-template-columns: repeat(auto-fill, minmax(320px, 1fr));
  gap: 35px;
  margin-top: 20px;
}

.no-results {
  text-align: center;
  padding: 80px 20px;
  background: #12131c;
  border-radius: 0px;
  border: 1px dashed rgba(204, 164, 59, 0.2);
  color: #a0a5b5;
  margin-top: 30px;
  font-size: 1rem;
  letter-spacing: 0.5px;
}

@media (max-width: 768px) {
  body {
    padding: 20px 0;
  }
  
  .app-header {
    flex-direction: column;
    align-items: flex-start;
    gap: 20px;
  }

  .counter-badge {
    align-self: flex-start;
  }
}
</style>