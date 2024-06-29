<template>
    <div class="login-container">
        <input type="text" v-model="username" placeholder="Username" @focus="clearError" />
        <input type="password" v-model="password" placeholder="Password" @focus="clearError" />
        <p v-if="errorMessage" class="error-message" v-html="errorMessage"></p>
        <div class="button-container">
            <button @click="login">Login</button>
            <button @click="register">Register</button>
        </div>
    </div>
</template>

<script>
export default {
    data() {
        return {
            username: '',
            password: '',
            errorMessage: ''
        }
    },
    methods: {
        clearError() {
            this.errorMessage = ''
        },
        validateInputs() {
            if (!this.username || !this.password) {
                this.errorMessage = 'Fill in Username and Password to Log In or Register'
                return false
            }
            return true
        },
        login() {
            this.clearError() // Clear error message before attempting login

            if (!this.validateInputs()) return

            const storedUser = localStorage.getItem(this.username)

            if (storedUser && JSON.parse(storedUser).password === this.password) {
                this.$router.push('/mainpage')
            } else {
                this.errorMessage =
                    'Username or Password Not Registered.<br>Try Log In With Already Registered Account.<br>Try Register to Make New Account.'
            }
        },
        register() {
            this.clearError() // Clear error message before attempting registration

            if (!this.validateInputs()) return

            const storedUser = localStorage.getItem(this.username)

            if (storedUser) {
                this.errorMessage =
                    'Username or Password Already Registered.<br>Try Log In With Already Registered Account.<br>Try Register to Make New Account.'
            } else {
                localStorage.setItem(this.username, JSON.stringify({ password: this.password }))
                this.$router.push('/mainpage')
            }
        }
    }
}
</script>

<style scoped>
.login-container {
    display: flex;
    flex-direction: column;
    align-items: center;
    justify-content: center;
    height: 100vh;
}

input {
    margin-bottom: 10px;
    padding: 10px;
    width: 300px;
    font-size: 16px;
    box-sizing: border-box;
}

.button-container {
    display: flex;
    justify-content: center;
    width: 100%;
}

button {
    margin: 5px;
    padding: 10px 20px;
}

.error-message {
    color: red;
    margin-bottom: 10px;
    text-align: center;
    width: 80%;
    line-height: 1.5;
}
</style>
