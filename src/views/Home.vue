<template>
  <div class="container">
    <Navbar />
    <p class="text-end">
      Welcome {{ userName }}
      <router-link :to="{ name: 'Profile' }">
        <button class="btn btn-info" type="button">Profile</button>
      </router-link>
    </p>
    <router-link :to="{ name: 'AddNewLocation' }">
      <button type="button" class="btn btn-primary">Add New Restaurant</button>
    </router-link>
    <br />
    <UserLocations :allLocations="listOfLocations" />
  </div>
</template>

<script>
import Navbar from "@/components/Header/Navbar.vue";
import UserLocations from "@/components/Locations/UserLocations.vue";
import { mapActions } from "vuex";
import axios from "axios";
export default {
  name: "Home",
  data() {
    return {
      userName: "",
      userId: "",
      listOfLocations: [],
    };
  },
  components: {
    Navbar,
    UserLocations,
  },
  async mounted() {
    let user = localStorage.getItem("user-info");
    if (!user) {
      this.redirectTo({ val: "Signup" });
    } else {
      this.userName = JSON.parse(user).name;
      this.userId = JSON.parse(user).id;
      let result = await axios.get(
        `http://localhost:3000/locations?userId=${this.userId}`
      );
      if (result.status == 200 && result.data.length > 0) {
        console.log(result.data);
        this.listOfLocations = result.data;
      }
    }
  },
  methods: {
    ...mapActions(["redirectTo"]),
  },
};
</script>
