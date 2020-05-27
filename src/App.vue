<template>
  <div id="app" class="small-container">
    <employee-form @add:employee="addEmployee" />
    <h1>Employees</h1>

    <employee-table :employees="employees" @delete:employee="deleteEmployee" @edit:employee="editEmployee"/>
  </div>
</template>

<script>
  // Import our components
  import EmployeeTable from '@/components/EmployeeTable.vue'
  import EmployeeForm from '@/components/EmployeeForm.vue'

  export default {
    name: 'app',
    components: {
      EmployeeTable,
      EmployeeForm
    },
    data(){
      return {
        // Array of employees, without APIs
        // employees: [
        //   {
        //     id: 1,
        //     name: 'Richard Hendricks',
        //     email: 'richard@piedpiper.com',
        //   },
        //   {
        //     id: 2,
        //     name: 'Bertram Gilfoyle',
        //     email: 'gilfoyle@piedpiper.com',
        //   },
        //   {
        //     id: 3,
        //     name: 'Dinesh Chugtai',
        //     email: 'dinesh@piedpiper.com',
        //   }
        // ]
        employees: []
      }
    },
    // "mounted" tells our component to perform the action once the component
    // is actually inserted to the DOM
    mounted() {
      this.getEmployees()
    },
    methods: {
      // Get all employees from https://jsonplaceholder.typicode.com/users
      async getEmployees(){
        try {
          // Since its an asynchronousMethod method, "await" is used which is non blocking and
          // the current thread can perform other actions during the wait
          const response = await fetch('https://jsonplaceholder.typicode.com/users')
          const data = await response.json()
          this.employees = data
        } catch(error) {
          console.error(error)
        }
      },
      // Add a new employee
      async addEmployee(employee){
        // Without APIs

        // For now, the ID isn't auto generated nor incremented, therefore we have to retrieve the last ID
        // and the new ID will be the last ID + 1
        // If the array's length is > 0, get the ID of the last item and store it in lastId
        // const lastId = this.employees.length > 0 ? this.employees[this.employees.length - 1].id : 0;
        // const id = lastId + 1;
        // Store the received employee in the variable newEmployee
        // const newEmployee = { ...employee, id };
        // Add the new employee to the array employees
        // this.employees = [...this.employees, newEmployee];

        // With APIs
        try {
          const response = await fetch('https://jsonplaceholder.typicode.com/users', {
            method: 'POST',
            body: JSON.stringify(employee),
            headers: { 'Content-Type' : 'application/json; charset=UTF-8'}
          })
          const data = await response.json()
          this.employees = [...this.employees, data]
        } catch(error) {
          console.error(error)
        }
        
      },
      // Without APIs

      // Delete an employee
      // deleteEmployee: function(id){
      //   // Filter the deleted row
      //   this.employees = this.employees.filter(employee => employee.id != id)
      // },

      // With APIs
      async deleteEmployee(id) {
        try {
          await fetch(`https://jsonplaceholder.typicode.com/users/${id}`, {
            method: 'DELETE'
          })
          this.employees = this.employees.filter(employee => employee.id !== id)
        } catch (error) {
          console.error(error)
        }
      },
      // Without APIs

      // editEmployee: function(id, updatedEmployee){
      //   // Map through the array of employees and update the correct employee
      //   this.employees = this.employees.map(employee =>
      //     employee.id === id ? updatedEmployee : employee
      //   )
      // }

      // With APIs
      async editEmployee(id, updatedEmployee) {
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

  button:hover,
  button:active,
  button:focus {
    background: #32a95d;
    border: 1px solid #32a95d;
  }
  .small-container {
    max-width: 760px;
  }
</style>