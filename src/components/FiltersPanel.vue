<script setup lang="ts">
import { computed } from "vue";

/* ---------- types ---------- */

export type Filters = {
  areaMin: number | null;
  areaMax: number | null;
  roomsMin: number | null;
  roomsMax: number | null;
  address: string;
};

/* ---------- props / emits ---------- */

const props = defineProps<{
  modelValue: Filters;
}>();

const emit = defineEmits<{
  (e: "update:modelValue", v: Filters): void;
}>();

/* ---------- v-model ---------- */

const model = computed({
  get: () => props.modelValue,
  set: (v: Filters) => emit("update:modelValue", v),
});

/* ---------- validation ---------- */

const areaInvalid = computed(
    () =>
        model.value.areaMin !== null &&
        model.value.areaMax !== null &&
        model.value.areaMin > model.value.areaMax
);

const roomsInvalid = computed(
    () =>
        model.value.roomsMin !== null &&
        model.value.roomsMax !== null &&
        model.value.roomsMin > model.value.roomsMax
);

/* ---------- reset ---------- */

function resetFilters() {
  model.value = {
    areaMin: null,
    areaMax: null,
    roomsMin: null,
    roomsMax: null,
    address: "",
  };
}
</script>

<template>
  <section class="panel">
    <div class="panel__row">
      <div class="field">
        <label>Площадь от</label>
        <input
            type="number"
            min="0"
            v-model.number="model.areaMin"
            :class="{ invalid: areaInvalid }"
        />
      </div>

      <div class="field">
        <label>Площадь до</label>
        <input
            type="number"
            min="0"
            v-model.number="model.areaMax"
            :class="{ invalid: areaInvalid }"
        />
      </div>
    </div>

    <p v-if="areaInvalid" class="hint">
      Площадь: «от» не может быть больше «до»
    </p>

    <div class="panel__row">
      <div class="field">
        <label>Комнат от</label>
        <input
            type="number"
            min="0"
            v-model.number="model.roomsMin"
            :class="{ invalid: roomsInvalid }"
        />
      </div>

      <div class="field">
        <label>Комнат до</label>
        <input
            type="number"
            min="0"
            v-model.number="model.roomsMax"
            :class="{ invalid: roomsInvalid }"
        />
      </div>
    </div>

    <p v-if="roomsInvalid" class="hint">
      Комнаты: «от» не может быть больше «до»
    </p>

    <div class="field">
      <label>Адрес</label>
      <input
          type="text"
          v-model="model.address"
          placeholder="Например: Түркістан"
      />
    </div>

    <div class="actions">
      <button type="button" class="btn" @click="resetFilters">
        Сбросить
      </button>
    </div>
  </section>
</template>

<style scoped>
.panel {
  background: #ffffff;
  border: 1px solid #e5e7eb;
  border-radius: 16px;
  padding: 16px;
  display: grid;
  gap: 14px;
}

.panel__row {
  display: grid;
  grid-template-columns: 1fr 1fr;
  gap: 12px;
}

.field {
  display: flex;
  flex-direction: column;
  gap: 6px;
}

label {
  font-size: 13px;
  font-weight: 500;
  color: #374151;
}

input {
  height: 42px;
  border-radius: 10px;
  border: 1px solid #d1d5db;
  padding: 0 12px;
  font-size: 14px;
  outline: none;
  transition: border-color 0.15s ease, box-shadow 0.15s ease;
}

input::placeholder {
  color: #9ca3af;
}

input:focus {
  border-color: #2563eb;
  box-shadow: 0 0 0 3px rgba(37, 99, 235, 0.15);
}

.invalid {
  border-color: #ef4444;
}

.hint {
  font-size: 12px;
  color: #b91c1c;
  margin: 0;
}

.actions {
  display: flex;
  justify-content: flex-end;
  margin-top: 6px;
}

.btn {
  height: 40px;
  padding: 0 18px;
  border-radius: 10px;
  border: none;
  background: #2563eb;
  color: #ffffff;
  font-size: 14px;
  font-weight: 500;
  cursor: pointer;
  transition: background 0.15s ease, transform 0.05s ease;
}

.btn:hover {
  background: #1d4ed8;
}

.btn:active {
  transform: translateY(1px);
}

/* ---------- mobile ---------- */
@media (max-width: 768px) {
  .panel__row {
    grid-template-columns: 1fr;
  }
}
</style>