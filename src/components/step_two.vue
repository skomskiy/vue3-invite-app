<script>
  import vSelect from '../components/v-select.vue'
  
  export default {
    name: "stepTwo",
    props: {
      locations: {
        default() {
          return []
        }
      },
      selected: {
        default: ''
      },
    },
    
    components: {
      vSelect,
    },

    emits: ['stepChanged', 'saveData'],

    data() {
      return {
        locationsAvailable: [
          { id: '1', name: 'Berlin', status: false },
          { id: '2', name: 'Poland Office', status: false },
          { id: '3', name: 'Venice Office', status: false },
          { id: '4', name: 'Mexico', status: false },
          { id: '5', name: 'USA Office', status: false },
          { id: '6', name: 'Ukraine Liyv Lukivska 7 Main Office', status: false },
          { id: '7', name: 'Canada', status: false },
        ],
        locations: [
          { name: 'Main Precoro US', value: 1 },
          { name: 'Main DOU US', value: 2 },
          { name: 'Main Epum US', value: 3 },
        ],
        selectedLocation: 'Select location',
        selectAll: false,
        errorLocation: false,
      }
    },

    methods: {
      validateForm() {
        this.errorLocation = this.selectedLocation === 'Select location';

        if (this.errorLocation) {
          return false;
        }

        return true;
      },

      optionSelect(option) {
        this.selectedLocation = option.name;
        // this.formData.company = this.selectedLocation;
      },

      toggleAll() {
        this.locationsAvailable.forEach(item => item.status = this.viewOnlyAll);
      },

      nextStep: function () {
        if (this.validateForm()) {
          this.saveFormData()
          this.$emit('save-data', this.locationsAvailable)
          this.$emit('step-changed', 3);
        }

        
      },

      saveFormData() {
        localStorage.setItem('locationsAvailable', JSON.stringify(this.locationsAvailable));
        localStorage.setItem('selectedLocation', JSON.stringify(this.selectedLocation));
      },
      
    },

    mounted() {
      const savedLocations = JSON.parse(localStorage.getItem('locationsAvailable'));
      if (savedLocations) {
        this.locationsAvailable = savedLocations;
      }

      const savedMain = JSON.parse(localStorage.getItem('selectedLocation'));
      if (savedMain) {
        this.selectedLocation = savedMain;
      }
    },
  }
</script>

<template>
  <div class="locations">
    <div class="locations__main">
      <span class="locations__main_title">Main location</span>
      <span class="locations__main_star">*</span>
      <vSelect
        :error="errorLocation"
        :options="locations"
        @select="optionSelect"
        :selected="selectedLocation"
      />
    </div>

    <input
      :id="8"
      class="checkbox__check"
      type="checkbox"
      v-model="viewOnlyAll"
      @change="toggleAll"
      >
    <label 
      class="checkbox__title"
      :for="8"
    >
      Select All Locations
    </label>
    <span class="locations__available_title">Available Locations</span>

    <ul class="listGrid">
      <li 
        class="checkbox"
        v-for="(location, id) in locationsAvailable" 
        :key="index"
      >
        <input
          :id="'checkbox-' + id"
          class="checkbox__check"
          type="checkbox" 
          v-model="location.status"
        >
        <label 
          class="checkbox__title"
          :for="'checkbox-' + id"
        >
          {{ location.name }}
        </label>
      </li>
    </ul>
  </div>

  <div class="container__down">
    <div 
      class="container__down_first container__down_second"
    >
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
