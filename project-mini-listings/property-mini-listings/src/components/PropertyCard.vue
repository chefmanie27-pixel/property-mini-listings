<template>
  <div class="property-card" :class="{ unavailable: !property.isAvailable }">
    <div class="image-container">
      <img :src="property.image" :alt="property.title" class="property-img" />
      <span v-if="!property.isAvailable" class="ribbon-badge">Not Available</span>
    </div>

    <div class="card-content">
      <div class="card-header">
        <span class="property-type">{{ property.type }}</span>
        <button
          @click="$emit('toggle-bookmark', property.id)"
          class="bookmark-btn"
          :class="{ active: isBookmarked }"
        >
          {{ isBookmarked ? "★ Bookmarked" : "☆ Bookmark" }}
        </button>
      </div>

      <h3 class="property-title">{{ property.title }}</h3>
      <p class="property-location">📍 {{ property.location }}</p>
      <p class="property-price">
        <strong>R {{ property.price }}</strong> / night
      </p>
    </div>
  </div>
</template>

<script>
export default {
  name: "PropertyCard",
  props: {
    property: {
      type: Object,
      required: true,
    },
    isBookmarked: {
      type: Boolean,
      default: false,
    },
  },
};
</script>

<style scoped>
.property-card {
  border: 1px solid #e0e0e0;
  border-radius: 8px;
  overflow: hidden;
  background: #fff;
  transition: transform 0.2s;
  position: relative;
}
.property-card:hover {
  transform: translateY(-5px);
}
.unavailable {
  opacity: 0.75;
  filter: grayscale(20%);
}
.image-container {
  position: relative;
  width: 100%;
  height: 200px;
}
.property-img {
  width: 100%;
  height: 100%;
  object-fit: cover;
}
.ribbon-badge {
  position: absolute;
  top: 10px;
  right: 10px;
  background-color: #e74c3c;
  color: white;
  padding: 5px 10px;
  font-size: 12px;
  font-weight: bold;
  border-radius: 4px;
}
.card-content {
  padding: 15px;
}
.card-header {
  display: flex;
  justify-content: space-between;
  align-items: center;
}
.property-type {
  font-size: 12px;
  text-transform: uppercase;
  color: #888;
  font-weight: bold;
}
.bookmark-btn {
  background: none;
  border: none;
  cursor: pointer;
  color: #555;
  font-size: 14px;
}
.bookmark-btn.active {
  color: #f1c40f;
  font-weight: bold;
}
.property-title {
  margin: 10px 0 5px;
  font-size: 18px;
}
.property-location {
  color: #666;
  font-size: 14px;
  margin-bottom: 10px;
}
.property-price {
  font-size: 16px;
  color: #2c3e50;
}
</style>