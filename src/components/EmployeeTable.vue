<template>
    <div id="employee-table">
        <!-- If there are no messages, the following message is displayed, else display all employees -->
        <p v-if="employees.length<1" class="empty-class">
            No Employees
        </p>
        <table v-else>
            <thead>
                <tr>
                    <th>Employee ID</th>
                    <th>Employee name</th>
                    <th>Employee email</th>
                    <th>Actions</th>
                </tr>
            </thead>
            <tbody>
                <tr v-for="employee in employees" :key="employee.id">
                    <td>{{ employee.id }}</td>
                    <!-- If we're in editing mode, therefore the variable "editing" is equal to the selected employee,
                    the fields name and email are set as text input,
                    and the button "Edit" is set to "Save" and the button "Delete" is set to "Cancel",
                    else their values are displayed -->
                    <td v-if="editing === employee.id">
                        <input type="text" v-model="employee.name" />
                    </td>
                    <td v-else>{{employee.name}}</td>
                    <td v-if="editing === employee.id">
                        <input type="text" v-model="employee.email" />
                    </td>
                    <td v-else>{{employee.email}}</td>
                    <td v-if="editing === employee.id">
                        <!-- The Save button submits new information to the editEmployee method
                        The Cancel button cancels editing by setting the variable "editing" to null -->
                        <button @click="editEmployee(employee)">Save</button>
                        <button class="muted-button" @click="cancelEdit(employee)">Cancel</button>
                    </td>
                    <td v-else>
                        <!-- The "Edit" button initializes the "editing" variable to the selected employee id
                        through the method "editMode"
                        The "Delete" button delets the selected employee -->
                        <button @click="editMode(employee)">Edit</button>
                        <button @click="$emit('delete:employee', employee.id)">Delete</button>
                    </td>
                </tr>
            </tbody>
        </table>
    </div>
</template>

<script>
  export default {
    name: 'employee-table',
    props: {
        employees: Array
    },
    data() {
        return {
            // When we're in editing mode, the following variable will be set equal 
            // to the employee ID we're editing
            editing: null
        }
    },
    methods: {
        editMode: function(employee){
            // Create a cachedEmployee to store the employee we're currently editing
            this.cachedEmployee = Object.assign({}, employee)
            this.editing = employee.id
        },
        editEmployee: function(employee){
            // If both fields name and email are empty, no changes are made
            if(employee.name === '' || employee.email === '') return
            // else, emit the event edit:employee to App.vue, by sending the employee ID
            // and the employee and reset "editing"
            this.$emit('edit:employee', employee.id, employee)
            this.editing = null
        },
        // Remove the employee id from the edit button and create a cached employee.
        cancelEdit: function(employee){
            Object.assign(employee, this.cachedEmployee)
            editing = null;
        }
    }
  }
</script>

<style scoped>
    button {
        margin: 0 0.5rem 0 0;
    }
    input {
        margin: 0;
    }
    .empty-table {
        text-align: center;
    }
</style>