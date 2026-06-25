<template>
  <div class="controls-bar">
    <div class="search-wrapper">
      <input
        :value="searchQuery"
        @input="$emit('update:searchQuery', $event.target.value)"
        type="text"
        placeholder="Search by title or location..."
        class="search-input"
      />
    </div>

    <div class="sort-wrapper">
      <select
        :value="sortBy"
        @change="$emit('update:sortBy', $event.target.value)"
        class="sort-dropdown"
      >
        <option value="default">Sort By Price</option>
        <option value="low-high">Price: Low to High</option>
        <option value="high-low">Price: High to Low</option>
      </select>
    </div>
  </div>
</template>

<script>
export default {
  name: "FilterControls",
  props: {
    searchQuery: {
      type: String,
      default: "",
    },
    sortBy: {
      type: String,
      default: "default",
    },
  },
};
</script>

<style scoped>
.controls-bar {
  display: flex;
  gap: 15px;
  margin: 20px 0;
  width: 100%;
}

.search-wrapper {
  flex: 1;
}

.search-input {
  width: 100%;
  padding: 12px 16px;
  font-size: 14px;
  border: 1px solid #ddd;
  border-radius: 6px;
  outline: none;
  transition: border-color 0.2s;
}

.search-input:focus {
  border-color: #3498db;
}

.sort-wrapper {
  min-width: 180px;
}

.sort-dropdown {
  width: 100%;
  padding: 12px 16px;
  font-size: 14px;
  border: 1px solid #ddd;
  border-radius: 6px;
  background-color: white;
  cursor: pointer;
  outline: none;
}

.sort-dropdown:focus {
  border-color: #3498db;
}

@media (max-width: 600px) {
  .controls-bar {
    flex-direction: column;
    gap: 10px;
  }

  .sort-wrapper {
    width: 100%;
  }
}
</style>