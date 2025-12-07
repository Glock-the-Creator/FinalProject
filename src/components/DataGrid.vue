<template>
  <!-- if api sends back nothing or we filtered it to nothing // If everything breaks it goes to this :( -->
  <div v-if="items.length === 0" class="alert alert-warning">
    No data returned from the API.
  </div>
  <!-- basic grid of cards -->
  <div v-else class="data-grid">
    <DataCard v-for="(thing, i) in items" :key="i" :item="thing"/>
  </div>
</template>

<script>
import DataCard from "./DataCard.vue";

export default {
  name: "DataGrid",
  components: {
    DataCard
  },

  props: {
    // as long as it's an array we're fine
    items: {
      type: Array,
      default() {
        return [];
      }
    }
  }
};
</script>

<style>
/* Grid wrapper for the cards */
.data-grid {
  display: grid;
  gap: 1rem;
  /* one column if you got a itty bitty screen */
  grid-template-columns: repeat(1, 1fr);
}

/* bump up to 3 columns on normal screens */
@media (min-width: 768px) {
  .data-grid {
    grid-template-columns: repeat(3, 1fr);
  }
}
</style>