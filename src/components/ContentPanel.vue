<template>
  <div class="content-panel rounded-4 p-3 h-100 d-flex flex-column">
    <div class="d-flex flex-column flex-md-row gap-2 mb-3">
      <div class="flex-grow-1 position-relative">
        <input
          v-model="search"
          type="text"
          class="form-control rounded-pill ps-5"
          placeholder="Search..."
        />
        <span class="search-icon">🔍</span>
      </div>
      <select class="form-select rounded-pill w-auto">
        <option>Sort by</option>
        <option>Name</option>
        <option>Rating</option>
      </select>
    </div>

    <div v-if="showSalons" class="flex-grow-1 overflow-auto pe-1">
      <SalonCard
        v-for="salon in filteredSalons"
        :key="salon.id"
        :salon="salon"
      />
    </div>

    <div v-else-if="showRings" class="flex-grow-1 overflow-auto pe-1">
      <RingsCard
        v-for="store in filteredStores"
        :key="store.id"
        :store="store"
      />
    </div>

    <div v-else-if="showShoes" class="flex-grow-1 overflow-auto pe-1">
      <ShoesCard v-for="shop in filteredShops" :key="shop.id" :store="shop" />
    </div>

    <div v-else-if="showHairdresser" class="flex-grow-1 overflow-auto pe-1">
      <HairdresserCard
        v-for="salon in filteredHairSalon"
        :key="salon.id"
        :store="salon"
      />
    </div>

    <div v-else-if="showCozmetic" class="flex-grow-1 overflow-auto pe-1">
      <CozmeticCard
        v-for="salon in filteredCozmetic"
        :key="salon.id"
        :store="salon"
      />
    </div>

    <div v-else-if="showMaidOfHonor" class="flex-grow-1 overflow-auto pe-1">
      <MaidOfHonorCard />
    </div>

    <div
      v-else
      class="d-flex flex-column justify-content-center align-items-center h-100 text-center"
    >
      <h4 class="mb-2 text-muted">This section will have its own page</h4>
      <p class="text-muted">
        Budget, notes, guests, chat ... will lead to different components.
      </p>
    </div>
  </div>
</template>

<script>
import SalonCard from "./SalonCard.vue";
import RingsCard from "./RingsCard.vue";
import ShoesCard from "./ShoesCard.vue";
import HairdresserCard from "./HairdresserCard.vue";
import CozmeticCard from "./CozmeticCard.vue";
import MaidOfHonorCard from "./MaidOfHonorCard.vue";

export default {
  name: "ContentPanel",
  components: {
    SalonCard,
    RingsCard,
    ShoesCard,
    HairdresserCard,
    CozmeticCard,
    MaidOfHonorCard,
  },
  props: {
    category: String,
    sub: String,
  },
  data() {
    return {
      search: "",
      salons: [
        {
          id: 1,
          name: "Angels-Wedding",
          rating: 4.9,
          address: "Zagrebačka ul. 1, Pula",
          status: "open",
          img: "https://picsum.photos/420/200?random=30",
        },
        {
          id: 2,
          name: "Vjenčanice Biba Rijeka",
          rating: 4.9,
          address: "Ul. Strossmayerova 2b, Rijeka",
          status: "open",
          img: "https://picsum.photos/420/200?random=31",
        },
        {
          id: 3,
          name: "Glamour Bridal",
          rating: 4.7,
          address: "Ilica 15, Zagreb",
          status: "open",
          img: "https://picsum.photos/420/200?random=32",
        },
      ],
      stores: [
        {
          id: 1,
          name: "Zlatarna Dodić",
          rating: 4.9,
          address: "Ilica 46, Zagreb",
          status: "open",
          img: "https://picsum.photos/420/200?random=40",
        },
        {
          id: 2,
          name: "Zlatarna Celje",
          rating: 4.9,
          address: "Praška ulica 8, Zagreb",
          status: "open",
          img: "https://picsum.photos/420/200?random=41",
        },
        {
          id: 3,
          name: "Prahir",
          rating: 4.7,
          address: "Ul. Vice Vukova 6, Zagreb",
          status: "open",
          img: "https://picsum.photos/420/200?random=42",
        },
      ],
      shops: [
        {
          id: 1,
          name: "Aldo",
          rating: 4.9,
          address: "Varšavska 46, Zagreb",
          status: "open",
          img: "https://picsum.photos/420/200?random=50",
        },
        {
          id: 2,
          name: "Mass",
          rating: 4.9,
          address: "Ilica 8, Zagreb",
          status: "open",
          img: "https://picsum.photos/420/200?random=51",
        },
        {
          id: 3,
          name: "Zooek",
          rating: 4.7,
          address: "Vukovarska ul. 6, Zagreb",
          status: "open",
          img: "https://picsum.photos/420/200?random=52",
        },
      ],
      hairSalon: [
        {
          id: 1,
          name: "Sasha",
          rating: 4.9,
          address: "Dalmatinska 4, Zagreb",
          status: "open",
          img: "https://picsum.photos/420/200?random=60",
        },
        {
          id: 2,
          name: "Vikler i Ruž",
          rating: 4.9,
          address: "Frankopanska 12",
          status: "open",
          img: "https://picsum.photos/420/200?random=61",
        },
        {
          id: 3,
          name: "Hairque",
          rating: 4.7,
          address: "Vukovarska ul. 6, Zagreb",
          status: "open",
          img: "https://picsum.photos/420/200?random=62",
        },
      ],
      cozmeticSalon: [
        {
          id: 1,
          name: "Estrella Beauty & academy",
          rating: 4.9,
          address: "Cetingradska 26, Zagreb",
          status: "open",
          img: "https://picsum.photos/420/200?random=60",
        },
        {
          id: 2,
          name: "Vikler i Ruž",
          rating: 4.9,
          address: "Frankopanska 12",
          status: "open",
          img: "https://picsum.photos/420/200?random=61",
        },
        {
          id: 3,
          name: "De luxe",
          rating: 4.7,
          address: "Ilica 35, Zagreb",
          status: "open",
          img: "https://picsum.photos/420/200?random=62",
        },
      ],
    };
  },
  computed: {
    showSalons() {
      return (
        (this.category === "bride" &&
          !["Rings", "Shoes", "Hairdresser", "Nails", "Maid of Honor"].includes(
            this.sub
          )) ||
        ["groom", "flowers", "church", "hall"].includes(this.category)
      );
    },
    showRings() {
      return this.category === "bride" && this.sub === "Rings";
    },
    showShoes() {
      return this.category === "bride" && this.sub === "Shoes";
    },
    showHairdresser() {
      return this.category === "bride" && this.sub === "Hairdresser";
    },
    showCozmetic() {
      return this.category === "bride" && this.sub === "Nails";
    },
    showMaidOfHonor() {
      return this.category === "bride" && this.sub === "Maid of Honor";
    },
    filteredSalons() {
      if (!this.search) return this.salons;
      return this.salons.filter((s) =>
        s.name.toLowerCase().includes(this.search.toLowerCase())
      );
    },
    filteredStores() {
      if (!this.search) return this.stores;
      return this.stores.filter((s) =>
        s.name.toLowerCase().includes(this.search.toLowerCase())
      );
    },
    filteredShops() {
      if (!this.search) return this.shops;
      return this.shops.filter((s) =>
        s.name.toLowerCase().includes(this.search.toLowerCase())
      );
    },
    filteredHairSalon() {
      if (!this.search) return this.hairSalon;
      return this.hairSalon.filter((s) =>
        s.name.toLowerCase().includes(this.search.toLowerCase())
      );
    },
    filteredCozmetic() {
      if (!this.search) return this.cozmeticSalon;
      return this.cozmeticSalon.filter((s) =>
        s.name.toLowerCase().includes(this.search.toLowerCase())
      );
    },
  },
};
</script>

<style scoped>
.content-panel {
  background: rgba(244, 231, 204, 0.95);
  border-radius: 1.5rem;
  height: 100%;
  min-height: calc(100vh - 130px);
  display: flex;
  flex-direction: column;
}
.search-icon {
  position: absolute;
  top: 50%;
  left: 15px;
  transform: translateY(-50%);
}
</style>
