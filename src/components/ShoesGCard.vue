<template>
  <div class="store-card mb-3 rounded-4 overflow-hidden" @click="$emit('read-more', storeg)">
    <div class="row g-0 h-100">
      <div class="col-12 col-md-4">
        <img :src="storeg.gallery && storeg.gallery.length ? storeg.gallery[0] : 'https://via.placeholder.com/420x200'" class="w-100 h-100 store-img" alt="store" />
      </div>

      <div class="col-12 col-md-8">
        <div class="p-3 h-100 d-flex flex-column">
          <div class="flex-grow-1 overflow-hidden" style="min-height:0">
            <h5 class="store-title">{{ storeg.name }}</h5>

            <div class="mb-1">
              <span class="text-warning">★ ★ ★ ★ ★</span>
              <small class="text-muted ms-2">{{ storeg.rating }}</small>
            </div>

            <p class="mb-0 small">📍 {{ storeg.address }}</p>
            <p class="mb-0 small">📞 {{ storeg.phone }}</p>
            <p class="mb-1 small">📧 {{ storeg.email }}</p>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: "ShoesGCard",
  props: {
    storeg: { type: Object, required: true },
  },
  emits: ["read-more"],
};
</script>

<style scoped>
.store-card {
  background: #fff;
  cursor: pointer;
  transition: transform 0.15s;
  box-shadow: 0 6px 14px rgba(0, 0, 0, 0.08);
  height: 230px;
}
.store-img {
  object-fit: cover;
  height: 230px;
}
.store-title {
  font-family: Georgia, serif;
}
</style>
