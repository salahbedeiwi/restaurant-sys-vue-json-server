<template>
  <div class="container">
    <Navbar />
    <form @click.prevent>
      <div class="row g-3 align-items-center">
        <div class="col-auto d-block mx-auto">
          <h1>Update Profile Info</h1>
        </div>
      </div>
      <div class="row g-3 align-items-center">
        <div class="col-auto d-block mx-auto">
          <div
            class="form-floating mb-3"
            :class="{ 'form-group-error': v$.name.$error }"
          >
            <input
              type="text"
              class="w250 form-control"
              id="floatNameIs"
              placeholder="Your Name Is"
              v-model.trim="name"
            />
            <label for="floatNameIs">Your Name Is</label>
            <span class="error-feedback" v-if="v$.name.$error">{{
              v$.name.$errors[0].$message
            }}</span>
          </div>
        </div>
      </div>
      <div class="row g-3 align-items-center">
        <div class="col-auto d-block mx-auto">
          <div
            class="form-floating mb-3"
            :class="{ 'form-group-error': v$.email.$error }"
          >
            <input
              type="email"
              class="w250 form-control"
              id="floatEmailIs"
              placeholder="Your Email Is"
              v-model.trim="email"
            />
            <label for="floatEmailIs">Your Email Is</label>
            <span class="error-feedback" v-if="v$.email.$error">{{
              v$.email.$errors[0].$message
            }}</span>
          </div>
        </div>
      </div>
      <div class="row g-3 align-items-center">
        <div class="col-auto d-block mx-auto">
          <span class="error-feedback" v-if="v$.pass.$error">{{
            v$.pass.$errors[0].$message
          }}</span>
          <div
            class="form-floating mb-3"
            :class="{ 'form-group-error': v$.pass.$error }"
          >
            <input
              type="password"
              class="w250 form-control"
              id="floatPassIs"
              placeholder="Your Password Is"
              v-model.trim="pass"
            />
            <label for="floatPassIs">Your Password Is</label>
            <span class="error-feedback" v-if="v$.pass.$error">{{
              v$.pass.$errors[0].$message
            }}</span>
          </div>
        </div>
      </div>
      <div class="row g-3 align-items-center">
        <div class="col-auto d-block mx-auto">
          <button
            type="submit"
            @click="updateProfileNow()"
            class="w250 btn btn-warning"
          >
            Update Now
          </button>
        </div>
      </div>

      <br />
      <div class="row g-3 align-items-center">
        <div class="col-auto d-block mx-auto error-feedback">
          {{ updateErr }}
        </div>
      </div>
    </form>
  </div>
</template>

<script>
import Navbar from "@/components/Header/Navbar.vue";
import axios from "axios";
import useValidate from "@vuelidate/core";
import { required, email, minLength } from "@vuelidate/validators";
import { mapActions } from "vuex";
export default {
  name: "UpdateProfile",
  components: {
    Navbar,
  },
  data() {
    return {
      v$: useValidate(),
      name: "",
      email: "",
      pass: "",
      userId: "",
      updateErr: "",
    };
  },
  validations() {
    return {
      name: { required, minLength: minLength(10) },
      email: { required, email },
      pass: { required, minLength: minLength(10) },
    };
  },
  mounted() {
    let user = localStorage.getItem("user-info");
    if (user) {
      this.name = JSON.parse(user).name;
      this.email = JSON.parse(user).email;
      this.pass = JSON.parse(user).pass;
      this.userId = JSON.parse(user).id;
    } else {
      this.redirectTo({ val: "Signup" });
    }
  },
  methods: {
    ...mapActions(["redirectTo"]),
    async updateProfileNow() {
      this.v$.$validate();
      if (!this.v$.$error) {
        console.log("Validated");
        //put
        let result = await axios.put(
          `http://localhost:3000/users/${this.userId}`,
          {
            name: this.name,
            email: this.email,
            pass: this.pass,
          }
        );
        console.log(result);
        if (result.status == 200) {
          console.log("Profile Is Updated Successfully");
          localStorage.setItem("user-info", JSON.stringify(result.data));
          this.redirectTo({ val: "Profile" });
          // this.$router.push({ name: "Profile", params: { pageTitle: "Profile Page" }});
        } else {
          console.warn("Profile Is Not Updated");
          this.userNotFoundErr = "Something went wrong, try again!";
        }
      } else {
        this.userNotFoundErr = "Something went wrong, refresh the page!";
      }
    },
  },
};
</script>

<style></style>
