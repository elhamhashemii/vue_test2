<template>
  <div id="app">
    <div class="flex-row header p-md">
      <p class="title">Pro Doctors</p>
      <p @click="showAddNewProfileHandler" class="add-new">+</p>
    </div>
    <div class="section" v-if="!showAddNewProfile">
      <div class="flex-row">
        <label class="label" for="filter">Find profile:</label>
        <input class="input" v-model="searchInput" />
        <div class="buttons">
          <button @click="sortAsc">▲</button>
          <button @click="sortDesc">▼</button>
        </div>
      </div>
      <ProfileCard
        v-for="profile in filteredProfiles"
        :key="profile.id"
        :profile="profile"
        class="profile"
        @likeDislikeHandler="likeDislikeHandler(profile)"
      />
    </div>
    <AddNewProfile
      v-if="showAddNewProfile"
      @closeAddNew="showAddNewProfile = false"
      :profiles="profiles"
    />
  </div>
</template>

<script>
import ProfileCard from "./components/ProfileCard";
import AddNewProfile from "./components/AddNewProfile.vue";

export default {
  name: "App",

  components: {
    ProfileCard,
    AddNewProfile,
  },

  data() {
    return {
      profiles: [
        {
          id: 1,
          name: "Wojciech",
          email: "wojciech@poz.pl",
          description: "Anaesthesiologist",
          likes: 34,
          userHasLikedProfile: false,
        },
        {
          id: 2,
          name: "Maria",
          email: "maria@poz.pl",
          description: "Radiologist",
          likes: 26,
          userHasLikedProfile: false,
        },
        {
          id: 3,
          name: "Anna",
          email: "anna@poz.pl",
          description: "Surgeon",
          likes: 53,
          userHasLikedProfile: false,
        },
      ],
      searchInput: "",
      showAddNewProfile: false,
    };
  },

  methods: {
    sortAsc() {
      this.profiles.sort(function (a, b) {
        return a.likes - b.likes;
      });
    },

    sortDesc() {
      this.profiles.sort(function (a, b) {
        return b.likes - a.likes;
      });
    },

    likeDislikeHandler(profile) {
      profile.userHasLikedProfile = !profile.userHasLikedProfile;
      if (profile.userHasLikedProfile) {
        profile.likes++;
      } else {
        profile.likes--;
      }
    },

    showAddNewProfileHandler() {
      this.showAddNewProfile = true;
    },
  },

  computed: {
    filteredProfiles() {
      return this.profiles.filter((profile) => {
        return profile.name
          .toLowerCase()
          .includes(this.searchInput.toLowerCase());
      });
    },
  },
};
</script>

<style>
@import url("https://fonts.googleapis.com/css2?family=Ubuntu:wght@300&display=swap");
* {
  margin: 0;
  padding: 0;
  box-sizing: border-box;
}
#app {
  font-family: "Ubuntu", sans-serif;
  text-align: center;
  color: #2c3e50;
  padding-bottom: 60px;
  position: relative;
  height: fit-content;
  background: linear-gradient(
    135deg,
    rgba(65, 184, 131, 0.9),
    rgba(52, 73, 94, 0.9)
  );

  font-size: 1.5em;
}

button {
  display: block;
  padding: 0.3em 1.5em;
  width: 100%;
  background-color: #41b883;
  border: 1px solid #41b883;
  border-radius: 12px;
  margin-left: 0.5rem;
  color: #fff;
  cursor: pointer;
  font-size: 0.5em;
  font-weight: 600;
}

.add-btn {
  padding: 1rem;
  margin: 0;
}

.content {
  display: flex;
}

.section {
  width: 100%;
  min-width: 500px;
  /* padding: 2em; */
  margin: 30px auto;
  position: relative;
  /* background: rgba(0, 0, 0, 0.15); */
}

@media screen and (min-width: 600px) {
  .section {
    width: 50vw;
    max-width: 15em;
  }
}

.section::before {
  content: "";
  position: absolute;
  top: -2px;
  left: 0;
  height: 2px;
  width: 100%;
  /* background: #35c3c1; */
}

.flex-row {
  display: flex;
}

.flex-column {
  display: flex;
  flex-direction: column;
  justify-content: start;
  align-items: flex-start;
}

.label {
  width: 80px;
  display: flex;
  align-items: center;
  justify-content: center;
  font-size: 12px;
  border-radius: 12px 0 0 12px;
  background: #f5f6f8;
  color: #0d7647;
  font-weight: bold;
}

.input {
  flex: 1;
  padding: 1em;
  border: 0;
  color: #8f8f8f;
  font-size: 1rem;
  outline: none;
  border-radius: 0 12px 12px 0;
  background-color: #b6efd5f9;
}

.invalid {
  color: red;
  /* border: 1px solid red; */
  background-color: rgb(255, 157, 157);
}

.invalid-msg {
  color: red;
  font-weight: bold;
  display: block;
  font-size: 0.8rem;
  margin: 0.5rem 0;
  background-color: rgb(255, 157, 157);
  border-radius: 8px;
  padding: 0.5rem;
}

.buttons {
  display: flex;
  /* box-shadow: 0 10px 10px rgba(0, 0, 0, 0.25), 0 5px 5px rgba(0, 0, 0, 0.22); */
  /* margin-top: 30px; */
}

.profile {
  margin-top: 20px;
}

.icons-note {
  margin-top: 30px;
  font-size: 10px;
}

.bg-white {
  background-color: #fff;
  margin-bottom: 1rem;
}

.p-md {
  padding: 0.75rem 0.5rem;
}

.checkbox {
  background: #f5f6f8;
  margin-top: 1.5rem;
  padding: 1rem;
  font-size: 0.8rem;
  text-align: start;
  border-radius: 12px;
  color: #0d7647;
  font-weight: bold;
}

.checkbox-item {
  color: blue;
  font-size: 0.8rem;
  font-weight: bold;
  margin: 0.25rem 2rem;
}

/* UI improvement */

.header,
.add-new-header {
  width: 100%;
  color: #2c3e50;
  justify-content: space-between;
  padding: 0.75rem 2rem;
  background-color: #79f7be;
}

.add-new-header {
  border-radius: 12px;
}

.m-md {
  margin-top: 1.5rem;
}

.add-new {
  cursor: pointer;
}

.justify-between {
  justify-content: space-between;
}

.specialization {
  border-radius: 12px;
}

.checkbox-label {
  color: #2a9867;
  font-weight: bold;
}

.title {
  color: rgb(23, 181, 107);
  font-weight: bold;
}
</style>
