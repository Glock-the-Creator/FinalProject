<template>
  <div class="app-background min-vh-100 d-flex align-items-center">
    <div class="container py-5">

      <!-- header + logo -->
      <header class="app-header mb-4">
        <div class="d-flex justify-content-between align-items-center">
          <div class="app-header-text">
            <h1 class="display-5 mb-1">Relativity &amp; Physics Papers</h1>
            <p class="small mb-0">
              Viewing a random set of scientific papers pulled from the CIS 255 API.
            </p>
          </div>

          <!-- logo -->
          <img src="/papers-logo.png" alt="Papers logo" class="app-logo d-none d-sm-block">
        </div>
      </header>

      <!-- shows login screen first -->
      <LoginForm v-if="!loggedIn" @login-success="onLogin"/>

      <!-- show the books and stuff -->
      <section v-else class="data-section">
        <div class="d-flex justify-content-between align-items-center mb-3">
          <h2 class="h4 mb-0">Found textbooks, theories and notes:</h2>

          <!-- button to just call the api again to find more stuff from it -->
          <button type="button" class="btn btn-outline-primary btn-sm" @click="getData">
            Find more
          </button>
        </div>

        <!-- basic status messages you're prob never gonna see cause they go away super fast -->
        <div v-if="loading" class="alert alert-info mb-3">
          Loading data from API...
        </div>

        <div v-else-if="errorMsg" class="alert alert-danger mb-3">
          {{ errorMsg }}
        </div>

        <!-- only show grid when we actually have data -->
        <DataGrid v-else :items="items"/>
      </section>
    </div>
  </div>
</template>

<script>
import LoginForm from "./components/LoginForm.vue";
import DataGrid from "./components/DataGrid.vue";

export default {
  name: "App",

  components: {
    LoginForm,
    DataGrid
  },

  data() {
    return {
      // you logged in dude?
      loggedIn: false,
      password: "",
      items: [],
      loading: false,
      errorMsg: ""
    };
  },

  methods: {
    // gets called when login form says "ok"
    onLogin(pw) {
      this.password = pw;
      this.loggedIn = true;
      this.getData();
    },

    // this actually hits the cis255 api
    async getData() {
      this.loading = true;
      this.errorMsg = "";
      this.items = [];

      try {
        const res = await fetch("https://cis255.vercel.app/api", {
          method: "POST",
          headers: {
            "Content-Type": "application/json"
          },
          body: JSON.stringify({ password: this.password })
        });

        if (!res.ok) {
          // if server is mad, just throw it away and let catch handle it
          throw new Error("Failed to fetch data. Status: " + res.status);
        }

        const data = await res.json();

        // api stuff
        this.items = this.fixData(data);
      } catch (err) {
        this.errorMsg = err && err.message
          ? err.message
          : "Something went wrong while getting data.";
      } finally {
        this.loading = false;
      }
    },

    // try to make sure we always end up with an array
    fixData(data) {
      // already an array? cool, just use it
      if (Array.isArray(data)) {
        return data;
      }

      // sometimes it comes back...Not always
      if (data && typeof data === "object") {
        const keys = Object.keys(data);
        if (keys.length === 1 && Array.isArray(data[keys[0]])) {
          return data[keys[0]];
        }
      }
      // worst case, wrap whatever this is so grid doesn't explode
      return [data];
    }
  }
};
</script>

<style>
/* main website background */
.app-background {
  background-color: #020617;
}

/* container a little thicker to let the cards breathe a bit */
.container {
  max-width: 1100px;
}

/* header bar */
.app-header {
  background-color: #0f172a; /* dark navy */
  color: #f9fafb;
  border-radius: 16px;
  padding: 1.25rem 1.75rem;
  box-shadow: 0 18px 35px rgba(15, 23, 42, 0.45);
}

/* White text */
.app-header .small {
  color: #e5e7eb;
}

/* title stuff */
.app-header-text h1 {
  font-weight: 600;
}

/* logo size */
.app-logo {
  height: 56px;
  width: auto;
}

/* Thing that holds all the stuff aka big box around little boxes */
.data-section {
  background-color: #020617;
  border-radius: 16px;
  padding: 1.75rem;
  border: 1px solid #1f2937;
  box-shadow: 0 12px 30px rgba(15, 23, 42, 0.6);
  color: #e5e7eb;
}

/* heading color inside big box */
.data-section h2 {
  color: #f9fafb;
}

/* alerts coloring */
.data-section .alert-info {
  background-color: #1d4ed8;
  border-color: #1d4ed8;
  color: #e5e7eb;
}

.data-section .alert-danger {
  background-color: #b91c1c;
  border-color: #b91c1c;
  color: #fef2f2;
}

/* Blue button refresh along with other button stuff */
.btn-primary {
  background-color: #2563eb;
  border-color: #2563eb;
}

.btn-primary:hover {
  background-color: #1d4ed8;
  border-color: #1d4ed8;
}

.btn-outline-primary {
  color: #93c5fd;
  border-color: #2563eb;
  background-color: transparent;
}

.btn-outline-primary:hover {
  background-color: #2563eb;
  border-color: #2563eb;
  color: #ffffff;
}
</style>