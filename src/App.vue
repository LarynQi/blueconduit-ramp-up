<template>
  <div id="app" class="small-container">
    <h1>Employees</h1>

    <employee-form @add:employee="addEmployee" />
    <employee-table
      :employees="employees"
      @delete:employee="deleteEmployee"
      @edit:employee="editEmployee"
    />
    <!-- <employee-table /> -->
  </div>
</template>

<script>
  import EmployeeTable from '@/components/EmployeeTable.vue'
// import EmployeeTable from './components/EmployeeTable.vue'
  import EmployeeForm from '@/components/EmployeeForm.vue'

  export default {
    name: 'app',
    components: {
      EmployeeTable,
      EmployeeForm
    },
    data() {
      return {
        employees: [],
      }
    },
    mounted() {
      this.getEmployees()
    },
    methods: {
      async addEmployee(employee) {
        try {
          const response = await fetch('https://jsonplaceholder.typicode.com/users', {
            method: 'POST',
            body: JSON.stringify(employee),
            headers: { 'Content-type': 'application/json; charset=UTF-8' },
          })
          const data = await response.json()
          this.employees = [...this.employees, data]
        } catch (error) {
          console.error(error)
        }
      },
      async getEmployees() {
        // const lastId = 
        //   this.employees.length > 0
        //     ? this.employees[this.employees.length - 1].id
        //     : 0;
        // const id = lastId + 1;
        // const newEmployee = { ...employee, id };
        // this.employees = [...this.employees, newEmployee];
        try {
          const response = await fetch('https://jsonplaceholder.typicode.com/users')
          const data = await response.json()
          this.employees = data
        } catch (error) {
          console.error(error)
        }
      },
      // deleteEmployee(id) {
      //   this.employees = this.employees.filter(
      //     employee => employee.id !== id
      //   )
      // },
      async deleteEmployee(id) {
        try {
          await fetch(`https://jsonplaceholder.typicode.com/users/${id}`, {
            method: "DELETE"
          });
          this.employees = this.employees.filter(employee => employee.id !== id);
        } catch (error) {
          console.error(error);
        }
      },
      async editEmployee(id, updatedEmployee) {
        // this.employees = this.employees.map(employee =>
        //   employee.id === id? updatedEmployee : employee
        // )
        try {
          const response = await fetch(`https://jsonplaceholder.typicode.com/users/${id}`, {
            method: 'PUT',
            body: JSON.stringify(updatedEmployee),
            headers: { 'Content-type': 'application/json; charset=UTF-8' },
          })
          const data = await response.json()
          this.employees = this.employees.map(employee => (employee.id === id ? data : employee))
        } catch (error) {
          console.error(error)
        }
      }
    }
  }

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
