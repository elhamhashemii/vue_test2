<template>
  <div class="section">
    <p class="add-new-header p-md flex-row">
        <span>add new profile</span>
        <span class="add-new" @click="$emit('closeAddNew')">x</span>
    </p>
    <div class="flex-row m-md">
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
    <div class="flex-row m-md">
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
    <div class="bg-white specialization">
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
          <label for="Surgeon" class="p-md checkbox-label">Surgeon</label>
        </div>

        <div class="checkbox-item">
          <input
            class="input"
            type="checkbox"
            id="radiologist"
            value="Radiologist"
            v-model="checkedSpecialisations"
          />
          <label for="Radiologist" class="p-md checkbox-label">Radiologist</label>
        </div>

        <div class="checkbox-item">
          <input
            class="input"
            type="checkbox"
            id="cardiologist"
            value="Cardiologist"
            v-model="checkedSpecialisations"
          />
          <label for="Cardiologist" class="p-md checkbox-label">Cardiologist</label>
        </div>
        <div class="checkbox-item">
          <input
            class="input"
            type="checkbox"
            id="psychiatrist"
            value="Psychiatrist"
            v-model="checkedSpecialisations"
          />
          <label for="Psychiatrist" class="p-md checkbox-label">Psychiatrist</label>
        </div>
        <div class="checkbox-item">
          <input
            class="input"
            type="checkbox"
            id="dermatologist"
            value="Dermatologist"
            v-model="checkedSpecialisations"
          />
          <label for="Dermatologist" class="p-md checkbox-label">Dermatologist</label>
        </div>
      </div>
    </div>
    <div v-if="formError" class="invalid-msg">
      Please complete the form correctly
    </div>
    <button class="add-btn" @click="addNewProfile">Add</button>
  </div>
</template>

<script>
export default {
  data() {
    return {
      name: "",
      email: "",
      specialisation: "",
      newProfile: {},
      formError: false,
      checkedSpecialisations: [],
    };
  },

  props: {
    profiles: {
      type: Array,
      required: true,
    },
  },

  emits: ["closeAddNew"],

  methods: {
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
        this.$emit("closeAddNew");
      } else {
        this.showErrorMessage();
        this.clearFileds();
      }
    },
  },

  computed: {
    validateName() {
      return /^[A-Za-z0-9 _]*[A-Za-z0-9][A-Za-z0-9 _]*$/.test(this.name);
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
