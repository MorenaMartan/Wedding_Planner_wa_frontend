<template>
  <div class="salon-card mb-3 rounded-4 overflow-hidden" @click="$emit('read-more', salon)">
    <div class="row g-0 h-100">
      <div class="col-12 col-md-4">
        <img
          :src="salon.gallery && salon.gallery.length ? salon.gallery[0] : 'https://via.placeholder.com/420x200'"
          class="w-100 h-100 salon-img"
          alt="salon"
        />
      </div>
      <div class="col-12 col-md-8">
        <div class="p-3 h-100 d-flex flex-column">
          <div class="flex-grow-1 overflow-hidden" style="min-height:0">
            <h5 class="mb-1 salon-title">{{ salon.name }}</h5>
            <div class="mb-1">
              <span class="text-warning">★ ★ ★ ★ ★</span>
              <small class="text-muted ms-2">{{ salon.rating }}</small>
            </div>
            <p class="mb-0 small">📍 {{ salon.address }}</p>
            <p class="mb-0 small">📞 {{ salon.phone }}</p>
            <p class="mb-1 small">📧 {{ salon.email }}</p>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: "SalonCard",
  props: {
    salon: { type: Object, required: true },
  },
  emits: ["read-more"],
};
</script>

<style scoped>
.salon-card {
  background: #fff;
  cursor: pointer;
  transition: transform 0.15s;
  box-shadow: 0 6px 14px rgba(0, 0, 0, 0.08);
  height: 230px;
}
.salon-img {
  object-fit: cover;
  height: 230px;
}
.salon-title {
  font-family: "Georgia", "Times New Roman", serif;
}
</style>
