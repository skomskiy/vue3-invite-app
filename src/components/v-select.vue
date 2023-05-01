<script>
  export default {
    name: "v-select",
    props: {
      options: {
        default() {
          return []
        }
      },
      selected: {
        default: ''
      },
      error: {
        default: false
      }
    },
    
    data() {
      return {
        areOptionsVisible: false
      }
    },
    
    methods: {
      selectOption(option) {
        this.$emit('select', option)
        this.areOptionsVisible = false;
      },

      hideSelect() {
        this.areOptionsVisible = false;
      },
    },
    
    mounted() {
      document.addEventListener('click', this.hideSelect.bind(this), true)
    },

    beforeDestroy() {
      document.removeEventListener('click', this.hideSelect)
    }
  }
</script>

<template>
  <div class="select">
    <p 
      class="select__title"
      :class="{ 'error': error }"
      @click="areOptionsVisible = !areOptionsVisible"
    >
      {{ selected }}
      <img src="../../public/img/Polygon2.svg" alt="Select company">
    </p>
    
    <div 
      class="select__options"
      v-if="areOptionsVisible"
    >
      <p
        class="select__option"
        v-for="option in options"
        :key="option.value"
        @click="selectOption(option)"
      >
        {{ option.name }}
    </p>
    </div>
  </div>
</template>
