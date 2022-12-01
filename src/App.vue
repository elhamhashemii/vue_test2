<template>
  <div id="app">
    <p>Profiles List</p>
    <div class="section">
      <div class="flex-row">
        <label class="label" for="filter">Find profile:</label>
        <input class="input" v-model="searchInput">
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
      />

      <div class="icons-note">
        Icons made by
        <a href="https://www.flaticon.com/authors/freepik" title="Freepik">Freepik</a> from
        <a href="https://www.flaticon.com/" title="Flaticon">www.flaticon.com</a>
      </div>
    </div>

    <div class="section">
      <p class="header">Add new profile:</p>
      <div class="flex-row">
        <label class="label">Name:</label>
        <input v-model="name" class="input" :class="name == '' || validateName ? '' : 'invalid' " id="name">
      </div>
      <span class="invalid-msg" v-if="name !== '' && !validateName">Name can only contains English characters</span>
      <div class="flex-row">
        <label class="label" for="filter">Email:</label>
        <input class="input"  :class="email == '' || validateEmail ? '' : 'invalid'" id="email" v-model="email">
      </div>
      <span class="invalid-msg" v-if="email !== '' && !validateEmail">Invalid email address</span>
      <div class="flex-row">
        <label class="label">Specialisation:</label>
        <input class="input" v-model="specialisation">
      </div>
      <div v-if="formError" class="invalid-msg">Please complete the form correctly</div>
      <button @click="addNewProfile">Add</button>
    </div>
  </div>
</template>

<script>
import ProfileCard from "./components/ProfileCard";

export default {
  name: "App",

  components: {
    ProfileCard
  },

  data() {
    return {
      profiles: [
        {
          id: 1,
          name: "Wojciech",
          email: "wojciech@poz.pl",
          description: "Anaesthesiologist",
          likes: 34
        },
        {
          id: 2,
          name: "Maria",
          email: "maria@poz.pl",
          description: "Radiologist",
          likes: 28
        },
        {
          id: 3,
          name: "Anna",
          email: "anna@poz.pl",
          description: "Surgeon",
          likes: 53
        }
      ],
      searchInput: "",
      name: "",
      email: "",
      specialisation: "",
      newProfile: {},
      formError: false
    };
  },

  methods: {
    sortAsc() {
      this.profiles.sort(function(a, b) {
        return a.likes - b.likes;
      });
    },

    sortDesc() {
      this.profiles.sort(function(a, b) {
        return b.likes - a.likes;
      });
    },

    clearFileds() {
        this.name = "",
        this.email = "",
        this.specialisation = ""
    },

    showErrorMessage() {
      this.formError = true
      setTimeout(() => {
        this.formError = false
      }, 3000);
    },

    addNewProfile() {
      let profilesLength = this.profiles.length;
      this.newProfile = {
        id: profilesLength + 1,
        name: this.name,
        email: this.email,
        description: "Description for now",
        likes: 0
      };
      if(this.validateName && this.validateEmail) {
        this.profiles.push(this.newProfile);
        this.clearFileds()
      } else {
        this.showErrorMessage()
        this.clearFileds()
        }
      }
    },


  computed: {
    filteredProfiles() {
      return this.profiles.filter((profile) => {
        return profile.name.toLowerCase().includes(this.searchInput.toLowerCase())
      })
    },

    validateName() {
      return /^[A-Za-z][A-Za-z0-9]*$/.test(this.name)
    },

    validateEmail() {
      return  /(.+)@(.+){2,}\.(.+){2,}/.test(this.email)
    }
  },

  // watch: {
  //   name(newVal, oldVal){
  //     if(newVal == "hello"){
  //       console.log(oldVal);
  //     }
  //   }
  // }

};
</script>

<style>
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
  background-color: #41B883;
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
  min-width: 300px;
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

.invalid{
  color: red;
  border: 1px solid red;
}

.invalid-msg{
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
</style>
