<template>
    <div class="row d-flex justify-content-center">
        <h1 class="text-center mb-5 mt-4">Already a member? Login to continue</h1>
        <div class="col-sm-6 mb-3 mb-sm-0">
            <form @submit=login>
                <div class="card text-center">
                    <div class="card-body">
                        <h5 class="card-title">Log In</h5>
                        <input placeholder="Library name..." class="mb-4 mt-4 inputStyle" required="true"
                            v-model="libraryName">
                        <br />
                        <input type="password" placeholder="Password..." class="mb-4 inputStyle" v-model="libraryPassword"
                            required="true">
                        <br />
                        <button class=" btn btn-dark mx-auto mt-2" type="submit">Log In</button>
                    </div>
                    <div class="alert alert-danger mx-auto" role="alert" v-if="errorAlert">
                        {{ errorMessage }}
                    </div>
                    <div class="alert alert-success mx-auto" role="alert" v-if="successAlert">
                        Successful login
                        <div class="spinner-border" role="status" v-if="successAlert"></div>
                    </div>
                </div>
            </form>
        </div>
    </div>
</template>

<script lang="ts">
import axios from 'axios'
import { ENDPOINT } from '@/env'

export interface ILoginData {
    name: string,
    password: string
}

export default {
    data() {
        return {
            libraryName: "",
            libraryPassword: "",
            errorMessage: "",
            errorAlert: false,
            successAlert: false,
            successMessage: ""
        }
    },
    methods: {
        login(event: any) {
            event.preventDefault();

            const loginLibraryData: ILoginData = {
                name: this.libraryName,
                password: this.libraryPassword
            }
            axios.post(`${ENDPOINT}/login`, loginLibraryData)
                .then(response => {
                    this.errorAlert = false;
                    this.successAlert = true;
                    window.localStorage.setItem("auth_token", response.data.Tokens.authToken);
                    window.localStorage.setItem("refresh_token", response.data.Tokens.refreshToken);
                    setTimeout(() => {
                        this.$router.push("/home")
                    }, 2000);
                    console.log(response.data);
                })
                .catch(error => {
                    this.successAlert = false;
                    this.errorAlert = true;
                    this.errorMessage = error.response.data.message;
                    console.log(error);
                })
        }
    }
}
</script>

<style scoped>
@import url('https://fonts.googleapis.com/css2?family=Montserrat:wght@200;400;500&display=swap');

h1,
h5,
button {
    font-family: Montserrat, system-ui, -apple-system, BlinkMacSystemFont, 'Segoe UI', Roboto, Oxygen, Ubuntu, Cantarell, 'Open Sans', 'Helvetica Neue', sans-serif;
    font-weight: 400;
}

.inputStyle {
    font-family: Montserrat, system-ui, -apple-system, BlinkMacSystemFont, 'Segoe UI', Roboto, Oxygen, Ubuntu, Cantarell, 'Open Sans', 'Helvetica Neue', sans-serif;
    width: 50%;
    border-top: none;
    border-left: none;
    border-right: none;
}
</style>