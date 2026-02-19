<template>
  <div class="cake-card mb-3 rounded-4 overflow-hidden" @click="$emit('read-more', cake)">
    <div class="row g-0 h-100">
      <div class="col-12 col-md-4">
        <img :src="cake.gallery && cake.gallery.length ? cake.gallery[0] : 'https://via.placeholder.com/420x200'" class="w-100 h-100 cake-img" alt="wedding cake" />
      </div>

      <div class="col-12 col-md-8">
        <div class="p-3 h-100 d-flex flex-column">
          <div class="flex-grow-1 overflow-hidden" style="min-height:0">
            <h5 class="cake-title">{{ cake.name }}</h5>

            <div class="mb-1">
              <span class="text-warning">★ ★ ★ ★ ★</span>
              <small class="text-muted ms-2">{{ cake.rating }}</small>
            </div>

            <p class="mb-0 small">📍 {{ cake.address }}</p>
            <p class="mb-0 small">📞 {{ cake.phone }}</p>
            <p class="mb-1 small">📧 {{ cake.email }}</p>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: "CakeCard",
  props: {
    cake: { type: Object, required: true },
  },
  emits: ["read-more"],
};
</script>

<style scoped>
.cake-card {
  background: #fff;
  cursor: pointer;
  transition: transform 0.15s;
  box-shadow: 0 6px 14px rgba(0, 0, 0, 0.08);
  height: 230px;
}
.cake-img {
  object-fit: cover;
  height: 230px;
}
.cake-title {
  font-family: Georgia, serif;
}
</style>
