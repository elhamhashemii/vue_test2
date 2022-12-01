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

      <div class="icons-note">
        Icons made by
        <a href="https://www.flaticon.com/authors/freepik" title="Freepik"
          >Freepik</a
        >
        from
        <a href="https://www.flaticon.com/" title="Flaticon"
          >www.flaticon.com</a
        >
      </div>
    </div>

    <div class="section">
      <p class="header">Add new profile:</p>
      <div class="flex-row">
        <label class="label">Name:</label>
        <input
          v-model="name"
          class="input"
          :class="name == '' || validateName ? '' : 'invalid'"
          id="name"
        />
      </div>
      <span class="invalid-msg" v-if="name !== '' && !validateName"
        >Name can only contains English characters</span
      >
      <div class="flex-row">
        <label class="label" for="filter">Email:</label>
        <input
          class="input"
          :class="email == '' || validateEmail ? '' : 'invalid'"
          id="email"
          v-model="email"
        />
      </div>
      <span class="invalid-msg" v-if="email !== '' && !validateEmail"
        >Invalid email address</span
      >
      <div class="bg-white">
        <div class="checkbox">Specialisation:</div>
        <div class="p-md flex-column">
          <div class="checkbox-item">
            <input
              class="input"
              type="checkbox"
              id="surgeon"
              value="Surgeon"
              v-model="checkedSpecialisations"
            />
            <label for="Surgeon" class="p-md">Surgeon</label>
          </div>

          <div class="checkbox-item">
            <input
              class="input"
              type="checkbox"
              id="radiologist"
              value="Radiologist"
              v-model="checkedSpecialisations"
            />
            <label for="Radiologist" class="p-md">Radiologist</label>
          </div>

          <div class="checkbox-item">
            <input
              class="input"
              type="checkbox"
              id="cardiologist"
              value="Cardiologist"
              v-model="checkedSpecialisations"
            />
            <label for="Cardiologist" class="p-md">Cardiologist</label>
          </div>
          <div class="checkbox-item">
            <input
              class="input"
              type="checkbox"
              id="psychiatrist"
              value="Psychiatrist"
              v-model="checkedSpecialisations"
            />
            <label for="Psychiatrist" class="p-md">Psychiatrist</label>
          </div>
          <div class="checkbox-item">
            <input
              class="input"
              type="checkbox"
              id="dermatologist"
              value="Dermatologist"
              v-model="checkedSpecialisations"
            />
            <label for="Dermatologist" class="p-md">Dermatologist</label>
          </div>
        </div>
      </div>
      <div v-if="formError" class="invalid-msg">
        Please complete the form correctly
      </div>
      <button @click="addNewProfile">Add</button>
    </div>
  </div>
</template>

<script>
import ProfileCard from "./components/ProfileCard";

export default {
  name: "App",

  components: {
    ProfileCard,
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
      name: "",
      email: "",
      specialisation: "",
      newProfile: {},
      formError: false,
      checkedSpecialisations: [],
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

    clearFileds() {
      (this.name = ""), (this.email = ""), (this.checkedSpecialisations = []);
    },

    showErrorMessage() {
      this.formError = true;
      setTimeout(() => {
        this.formError = false;
      }, 3000);
    },

    addNewProfile() {
      let profilesLength = this.profiles.length;
      this.newProfile = {
        id: profilesLength + 1,
        name: this.name,
        email: this.email,
        description: this.description,
        likes: 0,
        userHasLikedProfile: false,
      };
      if (this.validateName && this.validateEmail) {
        this.profiles.push(this.newProfile);
        this.clearFileds();
      } else {
        this.showErrorMessage();
        this.clearFileds();
      }
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
    },

    validateName() {
      return /^[A-Za-z][A-Za-z0-9]*$/.test(this.name);
    },

    validateEmail() {
      return /(.+)@(.+){2,}\.(.+){2,}/.test(this.email);
    },

    description() {
      return this.checkedSpecialisations.toString();
    },
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
