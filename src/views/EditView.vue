<template>
  <div
    class="edit-shell d-flex flex-column align-items-center justify-content-start"
  >
    <div class="d-flex align-items-center gap-3 mt-4">
      <img src="@/assets/logo.png" alt="Wedding Planner" class="top-logo" />

      <router-link to="/welcome" class="wedding-info-link text-decoration-none">
        <div class="wedding-info text-start">
          <h3 class="mb-0 fancy-name">Ivan &amp; Ivana</h3>
          <small class="date-text fancy-name">15.11.2026.</small>
        </div>
      </router-link>
    </div>

    <div class="edit-form mt-5 d-flex flex-column align-items-center">
      <label class="label-gold mb-2">Names</label>
      <input
        type="text"
        v-model="names"
        placeholder="Ivan & Ivana"
        class="form-input mb-3"
      />

      <label class="label-gold mb-2">Date</label>
      <input type="date" v-model="date" class="form-input mb-4" />

      <button class="btn btn-gold" @click="saveChanges">Save Changes</button>
    </div>
  </div>
</template>

<script>
export default {
  name: "EditView",
  data() {
    return {
      names: "Ivan & Ivana",
      date: "2026-11-15",
    };
  },
  methods: {
    saveChanges() {
      alert(`Saved: ${this.names} - ${this.date}`);
      this.$router.push("/welcome");
    },
  },
};
</script>

<style scoped>
.edit-shell {
  min-height: 100vh;
  background: #d4af37 url("@/assets/background.png") no-repeat center center;
  background-size: cover;
  color: #08182e;
  padding: 2rem;
  text-align: center;
}

.top-logo {
  height: 140px;
  filter: hue-rotate(190deg) saturate(2) brightness(0.55);
}

.wedding-info-link {
  color: inherit;
  transition: all 0.3s ease;
  border-radius: 10px;
  padding: 0.2rem 0.5rem;
}

.wedding-info-link:hover {
  background: rgba(8, 24, 46, 0.15);
  transform: translateY(-2px);
  cursor: pointer;
}

.fancy-name {
  font-family: "Italianno", cursive;
  font-size: 3rem;
  line-height: 1;
  color: #08182e;
}

.edit-form {
  width: 100%;
  max-width: 400px;
  backdrop-filter: blur(6px);
  background: rgba(212, 175, 55, 0.3);
  border-radius: 20px;
  padding: 2rem;
}

.label-gold {
  color: #08182e;
  font-weight: 600;
  letter-spacing: 0.05rem;
}

.form-input {
  width: 100%;
  padding: 0.6rem 1rem;
  border-radius: 30px;
  border: 1px solid #08182e;
  background: transparent;
  color: #08182e;
}

.form-input::placeholder {
  color: rgba(8, 24, 46, 0.7);
}

.btn-gold {
  background: #08182e;
  border: none;
  color: #d4af37;
  font-weight: 600;
  padding: 0.6rem 1.6rem;
  border-radius: 999px;
  transition: background 0.3s ease;
}

.btn-gold:hover {
  background: #0c274f;
  color: #d4af37;
}
.date-text {
  font-size: 30px;
}
</style>
