<script>
  import vSelect from './components/v-select.vue'
  import stepOne from './components/step_one.vue'
  import stepTwo from './components/step_two.vue'
  import stepThree from './components/step_three.vue'

  export default {
    name: 'app',
    components: {
      stepOne,
      vSelect,
      stepTwo,
      stepThree,
    },

    created() {
      this.clearFormData();
    },

    data() {
      return {
        step: 1,
        allData: []
      }
    },

    methods: {
      clearFormData() {
        localStorage.removeItem('formData');
        localStorage.removeItem('locationsAvailable');
        localStorage.removeItem('selectedLocation');
        localStorage.removeItem('items');
      },

      addObject(newObject) {
        this.allData.push(newObject)
        if (this.step === 3) {
          console.log(this.allData);

        }
      },

      updateStep(newStep) {
        this.step = newStep;
      },

      callChildMethod(newStep) {
        if (this.step === 1 && this.$refs.childComponentOne) {
          this.$refs.childComponentOne.nextStep()
        }
        if (this.step === 2 && this.$refs.childComponentTwo) {
          this.$refs.childComponentTwo.nextStep()
          this.updateStep(newStep)
        }
        if (this.step === 3 && this.$refs.childComponentThree) {
          this.$refs.childComponentThree.saveRoleData()
          this.updateStep(newStep)
        }
        else return
      }
    },
  }
</script>

<template>
  <div class="container">
    <div class="container__top">
      <h1 class="container__top_title">
        Invite User
      </h1>
      <button class="button__close">
        <img class="container__top_img" src="../public/img/Close.svg" alt="Close">
      </button>
    </div>
    
    <div class="container__buttons">
      <button 
        class="button"
        v-on:click="callChildMethod(1)"
        :disabled="this.step === 4"
      >
        <span 
          class="button__number"
          :class="{ active: step === 1 }"
        >1</span>
        <img 
          v-if="step > 1"
          class="button__number_done"
          src="../public/img/Done.svg" 
          alt="Done step">
        <span 
          class="button__title"
          :class="{ active: step === 1 }"
        >Main Info</span>
      </button>
      <button 
        class="button"
        v-on:click="callChildMethod(2)"
        :disabled="this.step === 4"
      >
        <span
          class="button__number"
          :class="{ active: step === 2 }"
        >2</span>
        <img 
          v-if="step > 2"
          class="button__number_done"
          src="../public/img/Done.svg" 
          alt="Done step">
        <span 
          class="button__title"
          :class="{ active: step === 2 }"
        >Available Locations</span>
      </button>
      <button 
        class="button"
        v-on:click="callChildMethod(3)"
        :disabled="this.step === 4"
      >
        <span 
          class="button__number"
          :class="{ active: step === 3 }"
        >3</span>
        <img 
          v-if="step > 3"
          class="button__number_done"
          src="../public/img/Done.svg" 
          alt="Done step">
        <span 
          class="button__title"
          :class="{ active: step === 3 }"
        >Roles</span>
      </button>
    </div>

    <div class="mainInfo" v-if="step === 1"
    >
      <stepOne
        @step-changed="updateStep"
        @save-data="addObject"
        ref="childComponentOne"
      />
    </div>

    <div class="availableLocation" v-if="step === 2">
      <stepTwo
        @step-changed="updateStep"
        @save-data="addObject"
        ref="childComponentTwo"
      />
    </div>

    <div class="roles" v-if="step > 2">
      <stepThree
        @step-changed="updateStep"
        @save-data="addObject"
        ref="childComponentThree"
      />
    </div>
  </div>
</template>
