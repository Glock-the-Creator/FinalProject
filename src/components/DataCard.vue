<template>
  <div
    class="card h-100 border-2"
    :class="[cardBorder, statusClass]">
    <div class="card-body">
      <!-- title up top -->
      <div class="d-flex justify-content-between align-items-center mb-2">
        <h3 class="card-title h6 mb-0">{{ cardTitle }}</h3>
      </div>
      <!-- show whatever fields we got back -->
      <ul class="small mb-0">
        <li v-for="(val, key) in stuffToShow" :key="key">
          <strong>{{ key }}:</strong> {{ formatValue(key, val) }}
        </li>
      </ul>
    </div>
  </div>
</template>

<script>
export default {
  name: "DataCard",

  props: ["item"],

  computed: {
    cardTitle() {
      if (this.item.title) return this.item.title;
      if (this.item.name) return this.item.name;
      return "Data Point";
    },

    // one "status" value we look at for colors
    // prefers item.status but will go to item.published if need be
    statusValue() {
      if (this.item.status !== undefined) {
        return this.item.status;
      }
      if (this.item.published !== undefined) {
        return this.item.published;
      }
      return undefined;
    },

    // clone the object and strip out stuff we don't want in the list
    stuffToShow() {
      const copy = { ...this.item };
      delete copy.id;      // id is kind of boring
      delete copy.status;  // using published for status instead
      return copy;
    },

    // set card border based on status/published
    cardBorder() {
      if (this.statusValue === true || this.statusValue === "true") {
        return "border-success";
      }
      if (this.statusValue === false || this.statusValue === "false") {
        return "border-danger";
      }
      // if no status or something weird
      return "border-secondary";
    },

    // extra class so we can tweak bg color a bit
    statusClass() {
      if (this.statusValue === true || this.statusValue === "true") {
        return "card-status-true";
      }
      if (this.statusValue === false || this.statusValue === "false") {
        return "card-status-false";
      }
      return "card-status-neutral";
    }
  },

  methods: {
    // If it's published "yes" if not "no"
    formatValue(key, val) {
      if (key === "published") {
        if (val === true || val === "true") {
          return "yes";
        }
        if (val === false || val === "false") {
          return "no";
        }
      }
      // everything else just shows whatever it already was
      return val;
    }
  }
};
</script>

<style>
/* background changes based on status whether or not it is published */
.card-status-true {
  background-color: #064e3b;
}

.card-status-false {
  background-color: #450a0a;
}

.card-status-neutral {
  background-color: #111827;
}

/* dark theme card */
.card {
  border-radius: 14px;
  border-color: #1f2937;
  color: #e5e7eb;
}
.card h3,
.card p,
.card li,
.card strong {
  color: #e5e7eb;
}
</style>