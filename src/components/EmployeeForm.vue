<template>
    <div id="employee-form">
        <form @submit.prevent="handleSubmit">
            <label>Employee name</label>
            <input type="text" :class="{'has-error': submitting && invalidName}"
                v-model="employee.name" @focus="clearStatus" @keypress="clearStatus"/>
            <label>Employee Email</label>
            <input type="text" :class="{'has-error': submitting && invalidEmail}"
                v-model="employee.email" @focus="clearStatus" />
            <!-- Error Message 
            An error message is displayed if the properties error and submitting are set to true-->
            <p v-if="error && submitting" class="error-message">❗Please fill out all required fields</p>
            <!-- Success Message 
            A success message is displayed if the property success is set to true-->
            <p v-if="success" class="success-message">✅ Employee successfully added</p>
            <button>Add Employee</button>
        </form>
    </div>
</template>

<script>
export default {
    name:'employee-form',
    data(){
        return {
            // Check if the form is currently being submitted
            submitting: false,
            // Check if something went wrong
            error: false,
            // If all conditions are validated
            success: false,
            employee: {
                name: '',
                email: ''
            }
        }
    },
    methods: {
        // The following function will set success and error to false by calling the method clearStatus(), 
        // set submitting to true. Finally, it'll check if one of the computed properties is true, then an error
        // is displayed. If not, added data in the form is submitted, and all the states are set back to default.
        handleSubmit: function(){
            this.submitting = true
            this.clearStatus()

            if (this.invalidName || this.invalidEmail) {
                this.error = true
                return
            }

            this.$emit('add:employee', this.employee)
            this.employee = {
            name: '',
            email: '',
            }
            this.error = false
            this.success = true
            this.submitting = false
        },
        clearStatus() {
            this.success = false
            this.error = false
        }
    },
    computed: {
        // Check if both fields aren't empty
        invalidName() {
            return this.employee.name === ''
        },

        invalidEmail() {
            return this.employee.email === ''
        },
    },
}
</script>

<style scoped>
    form {
        margin-bottom: 2rem;
    }

    [class*='-message'] {
        font-weight: 500;
    }

    .error-message {
        color: #d33c40;
    }

    .success-message {
        color: #32a95d;
    }
</style>