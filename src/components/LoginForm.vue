<template>
  <div class="row justify-content-center">
    <div class="col-md-6 col-lg-4">
      <div class="card shadow-sm login-card">
        <div class="card-body">
          <!-- logo -->
          <div class="text-center mb-3">
            <img src="/papers-logo.png" alt="Papers logo" class="login-logo">
          </div>
          <h2 class="h4 text-center mb-2">Login</h2>
            <p class="text-light small text-center mb-3">
              Type the password to see the papers.
                </p>
          <form @submit.prevent="submitForm">
            <div class="mb-3">
              <label for="password" class="form-label">Password</label>
              <input id="password" type="password" class="form-control" v-model="password" required/>
            </div>

            <!-- Password is wrong -->
            <div v-if="errorMessage" class="alert alert-danger py-2 small">
              {{ errorMessage }}
            </div>
            <button class="btn btn-primary w-100">
              Submit
            </button>
          </form>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: "LoginForm",

  data() {
    return {
      // pre-fill so you can literally just hit submit
      password: "cardinals",
      errorMessage: ""
    };
  },

  methods: {
    // super basic "auth" check
    submitForm() {
      if (this.password === "cardinals") {
        this.errorMessage = "";
        // tell App.vue that we made it in
        this.$emit("login-success", this.password);
      } else {
        this.errorMessage = "Incorrect password.";
      }
    }
  }
};
</script>

<style>
/* Login card styling */
.login-card {
  border-radius: 16px;
  border: 1px solid #1f2937;
  background-color: #0f172a;
  box-shadow: 0 12px 30px rgba(15, 23, 42, 0.6);
  color: #e5e7eb;
}

/* White text */
.login-card .text-muted,
.login-card .small,
.login-card p,
.login-card .form-label {
  color: #e5e7eb;
}

/* logo sizing */
.login-logo {
  height: 52px;
  width: auto;
}

/* Made inputs stay a lighter color so they're easier to type in */
.login-card .form-control {
  background-color: #f9fafb;
  border-color: #d1d5db;
  color: #111827;
}
</style>