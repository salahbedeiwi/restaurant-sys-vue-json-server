<template>
  <div class="container">
    <Navbar />
    <form @click.prevent>
      <div class="row g-3 align-items-center">
        <div class="col-auto d-block mx-auto">
          <h1>Your Profile</h1>
        </div>
      </div>
      <div class="row g-3 align-items-center">
        <div class="col-auto d-block mx-auto">
          <div class="form-floating mb-3">
            <input
              disabled
              type="text"
              class="w250 form-control"
              id="floatNameIs"
              placeholder="Your Name Is"
              v-model.trim="name"
            />
            <label for="floatNameIs">Your Name Is</label>
          </div>
        </div>
      </div>
      <div class="row g-3 align-items-center">
        <div class="col-auto d-block mx-auto">
          <div class="form-floating mb-3">
            <input
              disabled
              type="email"
              class="w250 form-control"
              id="floatEmailIs"
              placeholder="Your Email Is"
              v-model.trim="email"
            />
            <label for="floatEmailIs">Your Email Is</label>
          </div>
        </div>
      </div>
      <div class="row g-3 align-items-center">
        <div class="col-auto d-block mx-auto">
          <div class="form-floating mb-3">
            <input
              disabled
              type="password"
              class="w250 form-control"
              id="floatPassIs"
              placeholder="Your Password Is"
              v-model.trim="pass"
            />
            <label for="floatPassIs">Your Password Is</label>
          </div>
        </div>
      </div>
      <div class="row g-3 align-items-center">
        <div class="col-auto d-block mx-auto">
          <button
            type="submit"
            @click="updateProfile()"
            class="w250 btn btn-primary"
          >
            Update My Profile Info
          </button>
        </div>
      </div>
    </form>
  </div>
</template>

<script>
import Navbar from "@/components/Header/Navbar.vue";
import { mapActions } from "vuex";
export default {
  name: "ProfileForm",
  components: {
    Navbar,
  },
  data() {
    return {
      name: "",
      email: "",
      pass: "",
    };
  },
  mounted() {
    let user = localStorage.getItem("user-info");
    if (user) {
      this.name = JSON.parse(user).name;
      this.email = JSON.parse(user).email;
      this.pass = JSON.parse(user).pass;
    } else {
      this.redirectTo({ val: "Signup" });
    }
  },
  methods: {
    ...mapActions(["redirectTo"]),
    updateProfile() {
      this.redirectTo({ val: "UpdateProfile" });
    },
  },
};
</script>

<style></style>
