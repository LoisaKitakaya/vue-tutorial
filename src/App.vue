<template>
  <div id="app" class="small-container">
    <h1>Employees</h1>
    <EmployeeFormVue @add:employee="addEmployee" />
    <EmployeeTableVue
      v-bind:employees="employees"
      @delete:employee="deleteEmployee"
      @edit:employee="editEmployee"
    />
  </div>
</template>

<script>
import EmployeeTableVue from "./components/EmployeeTable.vue";
import EmployeeFormVue from "./components/EmployeeForm.vue";

export default {
  name: "App",
  components: {
    EmployeeTableVue,
    EmployeeFormVue,
  },
  data() {
    return {
      employees: [],
      url: "https://jsonplaceholder.typicode.com/users",
    };
  },
  methods: {
    async getEmployees() {
      try {
        const response = await fetch(this.url);
        const data = await response.json();
        this.employees = data;
      } catch (error) {
        console.error(error);
      }
    },
    async addEmployee(employee) {
      try {
        const response = await fetch(this.url, {
          method: "POST",
          body: JSON.stringify(employee),
          headers: { "Content-type": "application/json; charset=UTF-8" },
        });
        const data = await response.json();
        this.employees = [...this.employees, data];
      } catch (error) {
        console.error(error);
      }
    },
    async editEmployee(id, updatedEmployee) {
      try {
        const response = await fetch(`${this.url}/${id}`, {
          method: "PUT",
          body: JSON.stringify(updatedEmployee),
          headers: { "Content-type": "application/json; charset=UTF-8" },
        });
        const data = await response.json();
        this.employees = this.employees.map((employee) =>
          employee.id === id ? data : employee
        );
      } catch (error) {
        console.error(error);
      }
    },
    async deleteEmployee(id) {
      try {
        await fetch(`${this.url}/${id}`, {
          method: "DELETE",
        });
        this.employees = this.employees.filter(
          (employee) => employee.id !== id
        );
      } catch (error) {
        console.error(error);
      }
    },
  },
  mounted() {
    this.getEmployees();
  },
};
</script>

<style>
button {
  background: #009435;
  border: 1px solid #009435;
}

.small-container {
  max-width: 680px;
}
</style>
