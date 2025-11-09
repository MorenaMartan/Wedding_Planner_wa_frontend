<template>
  <div class="welcome-shell">
    <header
      class="topbar d-flex justify-content-between align-items-center px-3 px-lg-5 py-3"
    >
      <div class="d-flex align-items-center gap-3">
        <img src="@/assets/logo.png" alt="Wedding Planner" class="top-logo" />

        <router-link to="/edit" class="wedding-info-link text-decoration-none">
          <div class="wedding-info">
            <h3 class="mb-0 fancy-name">Ivan &amp; Ivana</h3>
            <small>{{ weddingDateDisplay }}</small>
          </div>
        </router-link>
      </div>

      <div class="remaining-pill">{{ daysLeftMessage }}</div>
    </header>

    <div class="container-fluid flex-grow-1">
      <div class="row h-100 g-3">
        <div class="col-12 col-lg-2 h-100">
          <LeftMenu
            :active="currentCategory"
            @select-category="onSelectCategory"
          />
        </div>

        <div class="col-12 col-lg-3 h-100">
          <SubMenu
            :category="currentCategory"
            :active-sub="currentSub"
            @select-sub="currentSub = $event"
          />
        </div>

        <div class="col-12 col-lg-7 h-100">
          <ContentPanel :category="currentCategory" :sub="currentSub" />
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import { ref, computed } from "vue";
import LeftMenu from "@/components/LeftMenu.vue";
import SubMenu from "@/components/SubMenu.vue";
import ContentPanel from "@/components/ContentPanel.vue";

export default {
  name: "WelcomeView",
  components: { LeftMenu, SubMenu, ContentPanel },
  setup() {
    const weddingDate = ref(new Date("2026-11-15"));

    const currentCategory = ref("bride");
    const currentSub = ref("Wedding dress");

    const weddingDateDisplay = computed(() =>
      weddingDate.value.toLocaleDateString("hr-HR", {
        day: "2-digit",
        month: "2-digit",
        year: "numeric",
      })
    );

    const daysLeft = computed(() => {
      const today = new Date();
      today.setHours(0, 0, 0, 0);
      const target = new Date(weddingDate.value);
      target.setHours(0, 0, 0, 0);
      const diffMs = target - today;
      return Math.ceil(diffMs / (1000 * 60 * 60 * 24));
    });

    const daysLeftMessage = computed(() => {
      if (daysLeft.value > 1) return `${daysLeft.value} days left`;
      if (daysLeft.value === 1) return "1 day left 💍";
      if (daysLeft.value === 0) return "Today is the big day! 💒";
      return "Wedding day has passed 💐";
    });

    const onSelectCategory = (cat) => {
      currentCategory.value = cat;
      const defaults = {
        bride: "Wedding dress",
        groom: "Suit",
        flowers: "Bouquet",
        church: "Ceremony",
        hall: "Venues",
      };
      currentSub.value = defaults[cat] || null;
    };

    return {
      weddingDate,
      weddingDateDisplay,
      daysLeftMessage,
      currentCategory,
      currentSub,
      onSelectCategory,
    };
  },
};
</script>

<style scoped>
.welcome-shell {
  min-height: 100vh;
  display: flex;
  flex-direction: column;
  gap: 1rem;
}

.topbar {
  background: rgba(245, 235, 218, 0.441);
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
  background: rgba(212, 175, 55, 0.15);
  transform: translateY(-2px);
  cursor: pointer;
}

.fancy-name {
  font-family: "Italianno", cursive;
  font-size: 3rem;
  line-height: 1;
  color: #2b2b2b;
}

.wedding-info small {
  color: #333;
}

.remaining-pill {
  background: #d4af37;
  color: #fff;
  padding: 0.5rem 2rem;
  border-radius: 999px;
  font-weight: 600;
}
</style>
