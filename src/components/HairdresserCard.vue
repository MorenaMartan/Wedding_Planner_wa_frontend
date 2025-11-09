<template>
  <div class="store-card mb-3 rounded-4 overflow-hidden">
    <div class="row g-0">
      <div class="col-12 col-md-4">
        <img :src="store.img" class="w-100 h-100 store-img" alt="store" />
      </div>
      <div class="col-12 col-md-8">
        <div class="p-3 h-100 d-flex flex-column justify-content-between">
          <div>
            <h5 class="store-title">{{ store.name }}</h5>
            <div class="mb-1">
              <span class="text-warning">★ ★ ★ ★ ★</span>
              <small class="text-muted ms-2">{{ store.rating }}</small>
            </div>
            <p class="mb-1">📍 {{ store.address }}</p>
            <p class="mb-2">⏱️ {{ store.status }}</p>
          </div>
          <div>
            <button class="btn btn-dark rounded-pill px-4 py-1">
              Read more...
            </button>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: "HairdresserCard",
  props: {
    store: { type: Object, required: true },
  },
};
</script>

<style scoped>
.store-card {
  background: #fff;
  box-shadow: 0 6px 14px rgba(0, 0, 0, 0.08);
}
.store-img {
  object-fit: cover;
  min-height: 150px;
}
.store-title {
  font-family: Georgia, serif;
}
</style>
