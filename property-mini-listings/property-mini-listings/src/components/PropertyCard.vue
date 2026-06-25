<template>
  <div class="property-card" :class="{ unavailable: !property.isAvailable }">
    <div class="image-container">
      <img :src="property.image" :alt="property.title" class="property-img" />
      <span v-if="!property.isAvailable" class="ribbon-badge"
        >Not Available</span
      >
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
      <p class="property-location">{{ property.location }}</p>

      
      <div class="card-footer">
        <p class="property-price">
          <strong>R {{ property.price }}</strong> <span>/ night</span>
        </p>
        <button
          @click="$emit('book-agent', property.id)"
          class="agent-btn"
          :disabled="!property.isAvailable"
        >
          Book Agent
        </button>
      </div>
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
  border: 1px solid rgba(204, 164, 59, 0.15); 
  border-radius: 4px; 
  overflow: hidden;
  background: #12131c; 
  transition:
    transform 0.3s cubic-bezier(0.25, 1, 0.5, 1),
    box-shadow 0.3s ease;
  position: relative;
}
.property-card:hover {
  transform: translateY(-5px);
  box-shadow:
    0 12px 30px rgba(0, 0, 0, 0.5),
    0 0 15px rgba(204, 164, 59, 0.1);
}
.unavailable {
  opacity: 0.5;
  filter: grayscale(60%);
}
.image-container {
  position: relative;
  width: 100%;
  height: 220px;
}
.property-img {
  width: 100%;
  height: 100%;
  object-fit: cover;
  transition: transform 0.5s ease;
}
.property-card:hover .property-img {
  transform: scale(1.03); 
}
.ribbon-badge {
  position: absolute;
  top: 12px;
  right: 12px;
  background-color: #7a221c; 
  color: #fff;
  padding: 4px 10px;
  font-size: 11px;
  font-weight: 600;
  letter-spacing: 0.5px;
  text-transform: uppercase;
}
.card-content {
  padding: 20px;
}
.card-header {
  display: flex;
  justify-content: space-between;
  align-items: center;
}
.property-type {
  font-size: 11px;
  text-transform: uppercase;
  color: #a0a5b5;
  font-weight: 600;
  letter-spacing: 1px;
}

.bookmark-btn {
  border: none;
  border-radius: 20px;
  background: linear-gradient(135deg, #0d0d0d 0%, #1a1a1a 50%, #cca43b 100%);
  padding: 6px 14px;
  cursor: pointer;
  color: #ffffff;
  font-size: 11px;
  font-weight: 500;
  letter-spacing: 0.5px;
  transition: all 0.3s ease;
  box-shadow: 0 2px 6px rgba(0, 0, 0, 0.3);
}
.bookmark-btn:hover {
  opacity: 0.9;
  box-shadow: 0 4px 10px rgba(204, 164, 59, 0.2);
}
.bookmark-btn.active {
  color: #ffe6a3;
  font-weight: bold;
}

.property-title {
  margin: 15px 0 6px;
  font-size: 1.15rem;
  font-weight: 400;
  color: #ffffff;
  letter-spacing: 0.5px;
}
.property-location {
  color: #a0a5b5;
  font-size: 13px;
  margin-bottom: 20px;
}

.card-footer {
  display: flex;
  justify-content: space-between;
  align-items: center;
  padding-top: 15px;
  border-top: 1px solid rgba(255, 255, 255, 0.05);
}
.property-price {
  font-size: 15px;
  color: #dfba73; 
}
.property-price span {
  color: #a0a5b5;
  font-size: 12px;
}

.agent-btn {
  background: transparent;
  border: 1px solid #cca43b;
  color: #cca43b;
  padding: 8px 16px;
  font-size: 12px;
  font-weight: 500;
  letter-spacing: 1px;
  text-transform: uppercase;
  cursor: pointer;
  transition: all 0.3s cubic-bezier(0.25, 1, 0.5, 1);
}
.agent-btn:hover:not(:disabled) {
  background: #cca43b;
  color: #0b0c10; 
  box-shadow: 0 4px 15px rgba(204, 164, 59, 0.3);
}
.agent-btn:disabled {
  border-color: rgba(255, 255, 255, 0.1);
  color: rgba(255, 255, 255, 0.2);
  cursor: not-allowed;
}
</style>
