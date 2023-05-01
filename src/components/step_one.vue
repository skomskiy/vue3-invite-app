<script>
  import vSelect from './v-select.vue' 

  export default {
    name: 'stepOne',

    components: {
      vSelect,
    },
            
    data() {
      return {
        options: [
          { name: 'Precoro', value: 1 },
          { name: 'DOU', value: 2 },
          { name: 'Epum', value: 3 },
        ],
        selectedCompany: 'Select conpany',
        formData: {
          firstName: '',
          lastName: '',
          email: '',
          phoneNumber: '',
          position: '',
          company: '',
        },
        errorTitle: false,
        errorLastName: false,
        errorEmail: false,
        errorPhoneNumber: false,
        errorPosition: false,
        errorCompany: false,
      }
    },

    emits: ['stepChanged', 'saveData'],

    methods: {
      validateForm() {
        const emailRegex = /\S+@\S+\.\S+/;
        const numberPattern = /^[^a-zA-Z]+$/;
        this.errorPhoneNumber = !numberPattern.test(this.formData.phoneNumber);


        this.errorTitle = this.formData.firstName.length < 2;
        this.errorLastName = this.formData.lastName.length < 2;
        this.errorEmail = !emailRegex.test(this.formData.email);
        this.errorPosition = this.formData.position.length < 2;
        this.errorCompany = this.formData.company.length < 2;



        if (this.errorTitle || this.errorLastName || this.errorEmail || this.errorPhoneNumber || this.errorPosition || this.errorCompany) {
          return false;
        }

        return true;
      },

      optionSelect(option) {
        this.selectedCompany = option.name;
        this.formData.company = this.selectedCompany;
      },

      saveFormData() {
        localStorage.setItem('formData', JSON.stringify(this.formData));
      },

      clearFormData() {
        localStorage.removeItem('formData');
        this.formData = {};
      },

      nextStep: function () {
        if (this.validateForm()) {
          this.saveFormData()
          this.$emit('save-data', this.formData)
          this.$emit('step-changed', 2);
        }
        // console.log(this.formData);
      },
    },

    mounted() {
      const savedFormData = JSON.parse(localStorage.getItem('formData'));
      if (savedFormData) {
        this.formData = savedFormData;
      }
    }
  };
</script>

<template>
  <form 
    action="#"
    method="post"
    class="form"
  >
    <div class="form__first">
      <label for="first-name" class="label">
        <span class="label__span">First Name:</span>
        <input
          v-model="formData.firstName"
          name="first-name"
          placeholder="First Name"
          type="text"
          class="label__input"
          :class="{ 'error': errorTitle }"
          minlength="2"
          id="first-name"
          required
        >
      </label>
      <label for="email" class="label">
        <span class="label__span">Email Address</span>
        <input
          v-model="formData.email"
          name="email"
          placeholder="Email"
          type="email"
          class="label__input"
          :class="{ 'error': errorEmail }"
          id="email"
          required
        >
      </label>
      <label for="position" class="label">
        <span class="label__span">Position</span>
        <input
          v-model="formData.position"
          name="position"
          placeholder="Position"
          type="text"
          class="label__input"
          :class="{ 'error': errorPosition }"
          id="position"
          required
        >
      </label>
    </div>
    <div 
      class="form__second"
      ref="myForm" @submit.prevent="submitForm">
      <label for="last-name" class="label">
        <span class="label__span">Last Name</span>
        <input
          v-model="formData.lastName"
          name="last-name"
          placeholder="Last Name"
          type="text"
          class="label__input"
          :class="{ 'error': errorLastName }"
          minlength="2"
          id="last-name"
          required
        >
      </label>
      <label for="phone-number" class="label">
        <span class="label__span">Phone number</span>
        <input
          v-model="formData.phoneNumber"
          name="number"
          placeholder="Phone number"
          type="text"
          class="label__input"
          :class="{ 'error': errorPhoneNumber }"
          id="phone-number"
          required
        >
      </label>
      <label class="label">
        <span class="label__span">Available in company</span>
        <vSelect
          :error="errorCompany"
          :options="options"
          @select="optionSelect"
          :selected="selectedCompany"
        />
      </label>
    </div>
  </form>

  <div class="container__down">
    <div 
      class="container__down_first"
    >
      <div class="toggle">
        <input type="checkbox" id="switch" class="toggle__input"/>
        <label for="switch" class="toggle__label">
        </label>
        <span for="switch" class="toggle__span">Active in all companies</span>
        <img 
          src="../../public/img/Vector.svg" 
          alt="more information"
          class="toggle__image"
        >
      </div>
  
      <button 
        :disabled="false"
        type="submit"
        v-on:click="nextStep"
        class="button__next"
      >
        Next step
      </button>
    </div>
  </div>
</template>