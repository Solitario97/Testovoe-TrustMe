<script setup lang="ts">
type Listing = {
  id: number;
  area: number;
  rooms: number;
  address: string;
  image: string;
};

defineProps<{ item: Listing }>();
</script>

<template>
  <article class="card">
    <img class="card__img" :src="item.image" :alt="`Фото: ${item.address}`" loading="lazy" />
    <div class="card__body">
      <div class="card__meta">
        <span class="badge">{{ item.rooms }} комн.</span>
        <span class="badge">{{ item.area }} м²</span>
      </div>
      <p class="card__addr">{{ item.address }}</p>
    </div>
  </article>
</template>

<style scoped>
.card { border: 1px solid #e5e7eb; border-radius: 14px; overflow: hidden; background: #fff; display: flex; flex-direction: column; }
.card__img { width: 100%; height: 180px; object-fit: cover; background: #f3f4f6; }
.card__body { padding: 12px; display: grid; gap: 8px; }
.card__meta { display: flex; gap: 8px; flex-wrap: wrap; }
.badge { font-size: 12px; padding: 6px 10px; border-radius: 999px; background: #f3f4f6; }
.card__addr { margin: 0; color: #111827; line-height: 1.35; }
</style>