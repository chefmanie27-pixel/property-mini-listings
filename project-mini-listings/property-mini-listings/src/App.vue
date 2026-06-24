<template>
  <div id="app" class="container">
    <header class="app-header">
      <div class="brand">
        <span class="brand-icon">🏠</span>
        <div>
          <h1>Homes & Beyond</h1>
          <p class="tagline">Short-term property rentals in Cape Town</p>
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
        No properties match your search criteria. Try a different search terms.
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
  font-family: "Segoe UI", Roboto, Helvetica, Arial, sans-serif;
}

body {
  background-color: #f9fbfd;
  color: #2c3e50;
  padding: 20px 0;
}

.container {
  max-width: 1140px;
  margin: 0 auto;
  padding: 0 20px;
}

.app-header {
  display: flex;
  justify-content: space-between;
  align-items: center;
  padding: 20px 0;
  border-bottom: 1px solid #e3e9ed;
  margin-bottom: 25px;
}

.brand {
  display: flex;
  align-items: center;
  gap: 12px;
}

.brand-icon {
  font-size: 2.5rem;
}

.app-header h1 {
  font-size: 1.75rem;
  font-weight: 700;
  color: #1a252f;
}

.tagline {
  font-size: 0.9rem;
  color: #7f8c8d;
}

.counter-badge {
  background-color: #ebf5fb;
  color: #2980b9;
  padding: 8px 16px;
  border-radius: 20px;
  font-size: 0.9rem;
  border: 1px solid #d4e6f1;
}

.properties-grid {
  display: grid;
  grid-template-columns: repeat(auto-fill, minmax(300px, 1fr));
  gap: 25px;
  margin-top: 10px;
}

.no-results {
  text-align: center;
  padding: 60px 20px;
  background: white;
  border-radius: 8px;
  border: 1px dashed #cbd5e1;
  color: #64748b;
  margin-top: 20px;
  font-size: 1.1rem;
}

@media (max-width: 600px) {
  .app-header {
    flex-direction: column;
    align-items: flex-start;
    gap: 15px;
  }

  .counter-badge {
    align-self: flex-start;
  }
}
</style>