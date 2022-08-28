<template>
    <div class="container">
        <form v-if="!secrets.length" action="#" @submit.prevent="handleLogin">
            <h3>Sign in for secrets!</h3>
            <div class="form-row">
                <input type="email" name="email" v-model="formData.email">
            </div>
            <div class="form-row">
                <input type="password" name="password" v-model="formData.password">
            </div>
            <div class="form-row">
                <button type="submit">Sign In</button>
            </div>
        </form>

        <div v-if="secrets.length" class="secrets">
            <ul>
                <li v-for="(secret, index) in secrets" :key="index">{{ secret.secret}}</li>
            </ul>
        </div>
    </div>
</template>

<script>
export default {
    data() {
        return {
            secrets: [],
            formData: {
                email: '',
                password: ''
            }
        }
    },
    methods: {
        handleLogin() {
            axios.get('/sanctum/csrf-cookie').then(response => {
                axios.post('/login', this.formData).then(response => {
                    this.getSecrets()
                })
            });
        },
        getSecrets() {
            axios.get('api/secrets').then(response => {
                this.secrets = response.data
            })
        }
    }
}
</script>

<style scoped>
.form-row {
    margin-bottom: 8px;
}
</style>
