<template>
  <div class="hairdresser-card mb-3 rounded-4 overflow-hidden" @click="$emit('read-more', hairdresser)">
    <div class="row g-0 h-100">
      <div class="col-12 col-md-4">
        <img
          :src="hairdresser.gallery && hairdresser.gallery.length ? hairdresser.gallery[0] : 'https://via.placeholder.com/420x200'"
          class="w-100 h-100 hairdresser-img"
          alt="hairdresser"
        />
      </div>
      <div class="col-12 col-md-8">
        <div class="p-3 h-100 d-flex flex-column">
          <div class="flex-grow-1 overflow-hidden" style="min-height:0">
            <h5 class="hairdresser-title">{{ hairdresser.name }}</h5>
            <div class="mb-1">
              <span class="text-warning">★ ★ ★ ★ ★</span>
              <small class="text-muted ms-2">{{ hairdresser.rating }}</small>
            </div>
            <p class="mb-0 small">📍 {{ hairdresser.address }}</p>
            <p class="mb-0 small">📞 {{ hairdresser.phone }}</p>
            <p class="mb-1 small">📧 {{ hairdresser.email }}</p>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: "HairdresserGCard",
  props: {
    hairdresser: { type: Object, required: true },
  },
  emits: ["read-more"],
};
</script>

<style scoped>
.hairdresser-card {
  background: #fff;
  cursor: pointer;
  transition: transform 0.15s;
  box-shadow: 0 6px 14px rgba(0, 0, 0, 0.08);
  height: 230px;
}
.hairdresser-img {
  object-fit: cover;
  height: 230px;
}
.hairdresser-title {
  font-family: Georgia, serif;
}
</style>
