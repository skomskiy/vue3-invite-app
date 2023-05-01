<script>
  export default {
    data() {
      return {
        items: [
          {
            name: 'Warehouse requests',
            options: {
              viewOnly: false,
              create: false,
              approve: false,
              management: {
                name: 'Configuration',
                status: false,
              }
            }
          },
          {
            name: 'Request for proposals',
            options: {
              viewOnly: false,
              create: false,
              approve: false,
              management: {
                name: 'Suppliers and items',
                status: false,
              }
            }
          },
          {
            name: 'Purchase orders',
            options: {
              viewOnly: false,
              create: false,
              approve: false,
              management: {
                name: 'Warehouse manager',
                status: false,
              }
            }
          },
          {
            name: 'Receipts',
            options: {
              viewOnly: false,
              create: false,
              approve: false,
              management: {
                name: 'Reports',
                status: false,
              }
            }
          },
          {
            name: 'Invoices',
            options: {
              viewOnly: false,
              create: false,
              approve: false,
              pay: false,
            }
          },
          {
            name: 'Expenses',
            options: {
              viewOnly: false,
              create: false,
              approve: false,
              pay: false,
              management: {
                name: 'Admin (Full access)',
                status: false,
              }
            }
          },
        ],

        viewOnlyAll: false,
        createAll: false,
        approveAll: false,
        payAll: false,
        allBellow: false,
        isDisabled: false,
        managementAll: false
      };
    },

    methods: {
      hasPayOption(item) {
        return Object.prototype.hasOwnProperty.call(item.options, 'pay');
      },

      hasManagementOption(item) {
        return Object.prototype.hasOwnProperty.call(item.options, 'management');
      },

      toggleAllViewOnly() {
        this.items.forEach(item => item.options.viewOnly = this.viewOnlyAll);
      },

      toggleAllCreate() {
        this.items.forEach(item => item.options.create = this.createAll);
      },

      toggleAllApprove() {
        this.items.forEach(item => item.options.approve = this.approveAll);
      },

      toggleAllPay() {
        this.items.forEach(item => {
          if (Object.prototype.hasOwnProperty.call(item.options, 'pay')) {
            item.options.pay = this.payAll;
          }
        });
      },

      toggleAllManagement() {
        this.items.forEach(item => {
          if (Object.prototype.hasOwnProperty.call(item.options, 'management')) {
            item.options.management.status = this.managementAll;
          }
        });
      },

      saveRoleData() {
        localStorage.setItem('items', JSON.stringify(this.items));
      },

      invite() {
        this.$emit('save-data', this.items)
        this.$emit('step-changed', 4);
        this.isDisabled = true;
      }
    },

    mounted() {
      const savedRoles = JSON.parse(localStorage.getItem('items'));
      if (savedRoles) {
        this.items = savedRoles;
      }
    },

    emits: ['stepChanged', 'saveData'],
  };
</script>

<template>
  <table class="table">
    <thead>
      <tr class="table__row table__row_head">
        <th class="table__headline table__headline_first">Access to:</th>
        <th class="table__headline">View Only</th>
        <th class="table__headline">Create</th>
        <th class="table__headline">Approve</th>
        <th class="table__headline">Pay</th>
        <th class="table__headline table__headline_last">
          Management:
          <img 
            src="../../public/img/Vector.svg" 
            alt="More info" 
            class="alert__img_management"
            >
        </th>
      </tr>
    </thead>
    <tbody class="table__body">
      <tr class="table__row">
        <td class="table__checkbox_items table__checkbox_all">All bellow</td>
        <td class="table__checkbox">
          <input
            class="checkbox__check"
            type="checkbox"
            v-model="viewOnlyAll"
            @change="toggleAllViewOnly"
            :disabled="isDisabled">
            
        </td>
        <td class="table__checkbox">
          <input
            :disabled="isDisabled"
            class="checkbox__check"
            type="checkbox"
            v-model="createAll"
            @change="toggleAllCreate">
        </td>
        <td class="table__checkbox">
          <input
            :disabled="isDisabled"
            class="checkbox__check"
            type="checkbox" 
            v-model="approveAll"
            @change="toggleAllApprove">
        </td>
        <td class="table__checkbox">
          <input
            class="checkbox__check"
            type="checkbox"
            v-model="payAll" 
            @change="toggleAllPay">
        </td>
        <td class="table__checkbox_last table__checkbox_all">
          <input
            :disabled="isDisabled"
            class="checkbox__check"
            type="checkbox"
            v-model="managementAll" 
            @change="toggleAllManagement"
          >
          All bellow
        </td>
      </tr>
      <tr class="table__row" v-for="(item, index) in items" :key="index">
        <td class="table__checkbox_items">{{ item.name }}</td>
        <td class="table__checkbox">
          <input 
            :disabled="isDisabled"
            class="checkbox__check"
            type="checkbox"
            v-model="item.options.viewOnly"
          >
        </td>
        <td class="table__checkbox">
          <input 
            :disabled="isDisabled"
            class="checkbox__check"
            type="checkbox"
            v-model="item.options.create"
          >
        </td>
        <td class="table__checkbox">
          <input
            :disabled="isDisabled"
            class="checkbox__check"
            type="checkbox"
            v-model="item.options.approve"
          >
        </td>
        <td class="table__checkbox" v-if="hasPayOption(item)">
          <input 
            :disabled="isDisabled"
            class="checkbox__check"
            type="checkbox" 
            v-model="item.options.pay"
          >
        </td>
        <td v-else></td>
        <td 
          v-if="hasManagementOption(item)" 
          class="table__checkbox_last"
          :class="{ table__checkbox_admin: item.options.management.name === 'Admin (Full access)' }"
        >
          <input 
            :disabled="isDisabled"
            class="checkbox__check"
            type="checkbox" 
            v-model="item.options.management.status"
          >
            {{ item.options.management.name }}
            <img 
              v-if="item.options.management.name === 'Admin (Full access)'" 
              src="../../public/img/Warning.svg" 
              alt="Warning"
              class="alert__img_warning"
              >
          </td>
      </tr>
    </tbody>
  </table>

  <div class="alert">
    <img 
      src="../../public/img/Vector.svg" 
      alt="info"
      class="alert__img"
    >
    <p class="alert__text">The user becomes a <a href="" v-on:click.prevent>Power user</a> if at least ONE of the following roles is selected:<br>Approve, View only, Configuration, Suppliers and Items, Budgets, Warehouse manager.</p>
  </div>

  <div class="container__down">
    <div 
      class="container__down_first container__down_second"
    >

    <button
      :disabled="isDisabled"
      type="submit"
      v-on:click="invite"
      class="button__next"
    >
      Invite User
    </button>
    </div>
  </div>
</template>
