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
      <!-- Part#1 , bug fixed by using profile.id instead of index -->
      <ProfileCard
        v-for="profile in filteredProfiles"
        :key="profile.id"
        :profile="profile"
        class="profile"
        @likeHandler="likeHandler(profile)"
        @dislikeHandler="dislikeHandler(profile)"
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
          userHasDislikedProfile: false,
        },
        {
          id: 2,
          name: "Maria",
          email: "maria@poz.pl",
          description: "Radiologist",
          likes: 26,
          userHasLikedProfile: false,
          userHasDislikedProfile: false,
        },
        {
          id: 3,
          name: "Anna",
          email: "anna@poz.pl",
          description: "Surgeon",
          likes: 53,
          userHasLikedProfile: false,
          userHasDislikedProfile: false,
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

    likeHandler(profile) {
      profile.userHasLikedProfile = !profile.userHasLikedProfile;
      if (profile.userHasLikedProfile && profile.userHasDislikedProfile) {
        profile.userHasDislikedProfile = false;
        profile.likes = profile.likes + 2;
      } else if (profile.userHasLikedProfile) {
        profile.likes++;
      } else {
        profile.likes--;
      }
    },

    // When profile.likes = 0, dislike action will make it -1
    // I wanted to fix this, but if I do, then it means :
    // the dislike will not be counted
    // (You disliked a profile, but it will look like you didn't do anything!)
    // SO, I didn't change this.
    dislikeHandler(profile) {
      profile.userHasDislikedProfile = !profile.userHasDislikedProfile;
      if (profile.userHasLikedProfile && profile.userHasDislikedProfile) {
        profile.userHasLikedProfile = false;
        profile.likes = profile.likes - 2;
      } else if (profile.userHasDislikedProfile) {
        profile.likes--;
      } else {
        profile.likes++;
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
@media screen and (max-width: 600px) {
  button {
    padding: 0.5rem;
  }
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
  margin: 30px auto;
  position: relative;
  padding: 1em;
  /* background: rgba(0, 0, 0, 0.15); */
}

@media screen and (min-width: 600px) {
  .section {
    width: 50vw;
    min-width: 350px;
    padding: 10px;
  }
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
  padding: 1rem 2rem;
  background-color: #aeffc6;
}

.add-new-header {
  border-radius: 12px;
}

.m-md {
  margin-top: 1.5rem;
}

.add-new {
  cursor: pointer;
  font-weight: bold;
  color: rgb(23, 181, 107);
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
