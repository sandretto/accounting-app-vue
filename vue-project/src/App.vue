<template>
<div class="container mt-2">
  <div class="card" style="width: 18rem; padding: 10px;">
    
<h3>Города:</h3>
      <select v-model="selectedCity" id="selectedCity">
          <option v-for="city in cities" 
          v-bind:key="city.id"
          v-bind:value="city">{{city.name}}</option>
      </select>
      <!-- <span v-if="selectedCity!==null">Выбран: {{selectedCity.name}}</span> -->
      <h3>Цеха:</h3>

      <select v-model="selectedDepartment">
          <option v-for="department in visibleDepartmentList" 
          v-bind:key="department.id"
          v-bind:value="department">{{department.name}}</option>
      </select>
      <!-- <span v-if="selectedDepartment!==null">Выбран: {{selectedDepartment.name}} цех</span> -->
      <h3>Сотрудники:</h3>
      <select v-model="selectedEmployee">
          <option v-for="employee in visibleEmployeeList" 
          v-bind:key="employee.id"
          v-bind:value="employee"> {{employee.firstName}} {{employee.lastName}}</option>
      </select>
      <!-- <span v-if="selectedEmployee!==null">Выбран: {{selectedEmployee.firstName}} {{selectedEmployee.lastName}}</span> -->
      
      <h3>Бригада:</h3>
      <p>{{ getTeam }}</p>
      <h3>Смена:</h3>
      <p>{{ getShift }}</p>
      <h3>Информация:</h3>
      <p>{{saveCookies()}}</p>
  </div>
</div>
</template>
<script>
import { uuid } from 'vue-uuid';
export default {
  data() {
    return {
    selectedCity: null,
    selectedDepartment: null,
    selectedEmployee: null,
    cityId: "",
    departmentId: "",
    employeeId: "",
    team: "",
    shift: "",
    cities: [
      {
        id: 1,
        name: "Париж",
      },
      {
        id: 2,
        name: "Флоренция",
      },
      {
        id: 3,
        name: "Кёльн",
      },
    ],
    departments: [
      {
        cityId: 1,
        id: 1,
        name: "Столярный",
      },
      {
        cityId: 1,
        id: 2,
        name: "Литейный",
      },
      {
        cityId: 2,
        id: 3,
        name: "Мебельный",
      },
      {
        cityId: 3,
        id: 4,
        name: "Швейный",
      },
    ],
    employees: [
      {
        departmentId: 2,
        firstName: "Жан-Поль",
        id: 1,
        isNightShift: false,
        lastName: "Мулер",
      },
      {
        departmentId: 2,
        firstName: "Жан-Пьер",
        id: 2,
        isNightShift: true,
        lastName: "Мулер",
      },
      {
        departmentId: 1,
        firstName: "Луи",
        id: 3,
        isNightShift: false,
        lastName: "Шарпентье",
      },
      {
        departmentId: 1,
        firstName: "Мишель",
        id: 4,
        isNightShift: true,
        lastName: "Шарпентье",
      },
      {
        departmentId: 3,
        firstName: "Джанкарло",
        id: 5,
        isNightShift: true,
        lastName: "Джепетто",
      },
      {
        departmentId: 3,
        firstName: "Пиноккио",
        id: 6,
        isNightShift: false,
        lastName: "Джепетто",
      },
      {
        departmentId: 4,
        firstName: "Марта",
        id: 7,
        isNightShift: false,
        lastName: "Шнайдер",
      },
      {
        departmentId: 4,
        firstName: "Грета",
        id: 8,
        isNightShift: true,
        lastName: "Шнайдер",
      },
    ],
  }},
  computed: {
    visibleDepartmentList: function () {
      if (this.selectedCity == null) return this.departments;
      var cId = this.selectedCity.id;
      return this.departments.filter(function (elem) {
        return elem.cityId == cId;
      });
    },

    visibleEmployeeList: function () {
      if (this.selectedDepartment == null) {
        return this.selectedCity == null ? this.employees : [];
      }
      var dId = this.selectedDepartment.id;
      return this.employees.filter(function (elem) {
        return elem.departmentId == dId;
      });
    },

    getTeam: function () {
      if (this.selectedEmployee == null) return "";
      return this.selectedEmployee.isNightShift == true
        ? "Вторая смена"
        : "Первая смена";
    },

    getShift: function () {
      if (this.selectedEmployee == null) return "";
      return this.selectedEmployee.isNightShift == true
        ? "c 20:00 до 8:00"
        : "c 8 до 20:00";
    },
  },
  methods: {
    setCookie: function (name, value, options = {}) {
      options = {
        path: '/',
        ...options
      };
    
      if (options.expires instanceof Date) {
        options.expires = options.expires.toUTCString();
      }
    
      let updatedCookie = name + "=" + value;
    
      for (let optionKey in options) {
        updatedCookie += "; " + optionKey;
        let optionValue = options[optionKey];
        if (optionValue !== true) {
          updatedCookie += "=" + optionValue;
        }
      }
    
      document.cookie = updatedCookie;
    },

    objToString: function (obj) {
    var str = '';
    for (var p in obj) {
      const has = Object.prototype.hasOwnProperty;
        if (has.call(obj, p)) {
            str += obj[p] + ', ';
        }
    }
    return str;
},
    saveCookies: function () {
      if (
        this.selectedCity == null ||
        this.selectedDepartment == null ||
        this.selectedEmployee == null
      )
        return "";
      else {

        var info = { 
          id: uuid.v1(), 
          city:  this.selectedCity.name,
          department: this.selectedDepartment.name,
          employee: this.selectedEmployee.firstName + ' ' +
            this.selectedEmployee.lastName,
          team: this.getTeam,
          shift: this.getShift
        };
        this.$cookies.set('info', info);
        return this.objToString(this.$cookies.get('info'));
      }
    } 
}
}
</script>