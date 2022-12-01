<template>
  <div id="app">
    <p>Profiles List</p>
    <div class="section">
      <div class="flex-row">
        <label class="label" for="filter">Find profile:</label>
        <input class="input" v-model="searchInput" />
      </div>
      <div class="buttons">
        <button @click="sortAsc">▲</button>
        <button @click="sortDesc">▼</button>
      </div>
      <ProfileCard
        v-for="profile in filteredProfiles"
        :key="profile.id"
        :profile="profile"
        class="profile"
        @likeDislikeHandler="likeDislikeHandler(profile)"
      />
    </div>
    <AddNewProfile :profiles="profiles" />
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
          likes: 28,
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
  },

  computed: {
    filteredProfiles() {
      return this.profiles.filter((profile) => {
        return profile.name
          .toLowerCase()
          .includes(this.searchInput.toLowerCase());
      });
    }
  },
};
</script>

<style>
* {
  margin: 0;
  padding: 0;
  box-sizing: border-box;
}
#app {
  font-family: "Roboto", helvetica, arial, sans-serif;
  text-align: center;
  color: #2c3e50;
  padding: 60px;
  display: flex;
  align-items: center;
  justify-content: center;
  flex-direction: column;
  position: relative;

  background: linear-gradient(
    135deg,
    rgba(65, 184, 131, 0.9),
    rgba(52, 73, 94, 0.9)
  );

  font-size: 1.5em;
}

button {
  display: block;
  padding: 1em;
  width: 100%;
  background-color: #41b883;
  border: 1px solid;
  color: #fff;
  cursor: pointer;
  font-size: 0.75em;
  font-weight: 600;
  text-shadow: 0 1px 0 rgba(black, 0.2);
}

.content {
  display: flex;
}

.section {
  width: 100%;
  min-width: 500px;
  padding: 2em;
  margin-top: 30px;
  position: relative;
  background: rgba(0, 0, 0, 0.15);
}

@media screen and (min-width: 600px) {
  .section {
    width: 50vw;
    max-width: 15em;
  }
}

.header {
  color: #fff;
}

.section::before {
  content: "";
  position: absolute;
  top: -2px;
  left: 0;
  height: 2px;
  width: 100%;
  background: #35c3c1;
}

.flex-row {
  display: flex;
  margin-top: 1em;
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

  background: #f5f6f8;
}

.input {
  flex: 1;
  padding: 1em;
  border: 0;
  color: #8f8f8f;
  font-size: 1rem;
  outline: none;
}

.invalid {
  color: red;
  border: 1px solid red;
}

.invalid-msg {
  color: red;
  display: block;
  font-size: 0.8rem;
  margin: 0.5rem 0;
}

.buttons {
  display: flex;
  box-shadow: 0 10px 10px rgba(0, 0, 0, 0.25), 0 5px 5px rgba(0, 0, 0, 0.22);
  margin-top: 30px;
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
  padding: 0.5rem 1rem;
  font-size: 0.8rem;
  text-align: start;
}

.checkbox-item {
  color: blue;
  font-size: 0.8rem;
  font-weight: bold;
  margin: 0.25rem 2rem;
}
</style>
