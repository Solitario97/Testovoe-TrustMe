<script setup lang="ts">
import { computed, ref } from "vue";
import listings from "./data/listings.json";
import FiltersPanel, { type Filters } from "./components/FiltersPanel.vue";
import ListingCard from "./components/ListingCard.vue";
import { normalizeText } from "./utils/normalize";

type Listing = {
  id: number;
  area: number;
  rooms: number;
  address: string;
  image: string;
};

const all = listings as Listing[];

const filters = ref<Filters>({
  areaMin: null,
  areaMax: null,
  roomsMin: null,
  roomsMax: null,
  address: "",
});

const showFilters = ref(false);

/* filtering */
const filtered = computed(() => {
  const q = normalizeText(filters.value.address);

  return all.filter((x) => {
    const areaOk =
        (filters.value.areaMin === null || x.area >= filters.value.areaMin) &&
        (filters.value.areaMax === null || x.area <= filters.value.areaMax);

    const roomsOk =
        (filters.value.roomsMin === null || x.rooms >= filters.value.roomsMin) &&
        (filters.value.roomsMax === null || x.rooms <= filters.value.roomsMax);

    const addrOk =
        q === "" || normalizeText(x.address).includes(q);

    return areaOk && roomsOk && addrOk;
  });
});
</script>
<template>
  <div class="page">
    <header class="top">
      <h1 class="title">Поиск квартир</h1>
      <p class="sub">Астана</p>
    </header>

    <!-- mobile bar -->
    <div class="mobile-bar">
      <button class="filters-btn" @click="showFilters = true">
        Фильтры
      </button>

      <div class="count">
        Найдено: <b>{{ filtered.length }}</b>
      </div>
    </div>

    <main class="layout">
      <!-- desktop filters -->
      <aside class="filters desktop-only">
        <FiltersPanel v-model="filters" />
      </aside>

      <section class="content">
        <div v-if="filtered.length === 0" class="empty">
          Ничего не найдено
        </div>

        <div v-else class="grid">
          <ListingCard
              v-for="item in filtered"
              :key="item.id"
              :item="item"
          />
        </div>
      </section>
    </main>

    <!-- mobile filters overlay -->
    <div v-if="showFilters" class="overlay">
      <div class="sheet">
        <div class="sheet-head">
          <b>Фильтры</b>
          <button @click="showFilters = false">✕</button>
        </div>

        <FiltersPanel v-model="filters" />

        <button class="apply" @click="showFilters = false">
          Показать {{ filtered.length }}
        </button>
      </div>
    </div>
  </div>
</template>

<style scoped>
.page {
  max-width: 1200px;
  margin: 0 auto;
  padding: 16px;
}

.title {
  font-size: 20px;
  margin: 0;
}

.sub {
  color: #6b7280;
  font-size: 13px;
}

.layout {
  display: grid;
  grid-template-columns: 0.5fr 1fr;
  gap: 16px;
}

.filters {
  position: sticky;
  top: 16px;
}

.grid {
  display: grid;
  grid-template-columns: repeat(2, 1fr);
  gap: 14px;
}

.mobile-bar {
  display: none;
}

/* ---------- mobile ---------- */
@media (max-width: 980px) {
  .layout {
    grid-template-columns: 1fr;
  }

  .desktop-only {
    display: none;
  }

  .grid {
    grid-template-columns: 1fr;
  }

  .mobile-bar {
    display: flex;
    justify-content: space-between;
    align-items: center;
    gap: 12px;
    margin-bottom: 12px;
  }

  .filters-btn {
    padding: 8px 14px;
    border-radius: 10px;
    border: 1px solid #e5e7eb;
    background: #fff;
    font-size: 14px;
  }
}

/* ---------- overlay ---------- */
.overlay {
  position: fixed;
  inset: 0;
  background: rgba(0,0,0,0.4);
  z-index: 50;
}

.sheet {
  position: absolute;
  bottom: 0;
  left: 0;
  right: 0;
  background: #fff;
  border-radius: 20px 20px 0 0;
  padding: 16px;
  max-height: 85vh;
  overflow-y: auto;
}

.sheet-head {
  display: flex;
  justify-content: space-between;
  align-items: center;
  margin-bottom: 12px;
}

.apply {
  width: 100%;
  margin-top: 16px;
  padding: 12px;
  border-radius: 12px;
  background: #111827;
  color: #fff;
  font-size: 15px;
}
</style>