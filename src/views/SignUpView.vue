<template>
    <div class="row d-flex justify-content-center">
        <h1 class="text-center mb-5 mt-4">¡We are happy to have you here!</h1>
        <div class="col-sm-6 mb-3 mb-sm-0">
            <form @submit=registerNewLibrary>
                <div class="card text-center">
                    <div class="card-body">
                        <h5 class="card-title">Register your account</h5>
                        <input placeholder="Library name..." class="mb-4 mt-4 inputStyle" v-model="libraryName"
                            required="true">
                        <br />
                        <input type="password" placeholder="Password..." class="mb-4 inputStyle" v-model="libraryPassword"
                            required="true">
                        <br />
                        <input placeholder="Location..." class="mb-4 inputStyle" v-model="libraryLocation" required="true">
                        <br />
                        <button class=" btn btn-dark mx-auto" type="submit">Register</button>
                        <div class="alert alert-success mt-3" role="alert" v-if="successMessage">
                            ¡Your library was created successfully!
                        </div>
                        <div class="spinner-border" role="status" v-if="successSpinner"></div>
                        <div class="alert alert-danger mt-3" role="alert" v-if="errorMessage">
                            There is already a library with that name
                        </div>
                    </div>
                </div>
            </form>
        </div>
    </div>
</template>

<script lang="ts">
import axios from "axios";
import { ENDPOINT } from "../env";
interface ICreateLibrary {
    name: string,
    password: string,
    location: string
}

export default {
    data() {
        return {
            libraryName: "",
            libraryPassword: "",
            libraryLocation: "",
            successMessage: false,
            errorMessage: false,
            successSpinner: false
        }
    },
    methods: {
        registerNewLibrary(event: any) {
            event.preventDefault();

            const libraryData: ICreateLibrary = { // add interface
                name: this.libraryName,
                password: this.libraryPassword,
                location: this.libraryLocation
            }
            axios.post(`${ENDPOINT}/create-library`, libraryData)
                .then(response => {
                    this.libraryName = "";
                    this.libraryPassword = "";
                    this.libraryLocation = ""
                    this.successMessage = true;
                    this.errorMessage = false;
                    this.successSpinner = true;
                    setTimeout(() => {
                        this.$router.push("/login")
                    }, 3000);

                })
                .catch(error => {
                    const errString: string = error.response.data.message;
                    if (errString.startsWith("Duplicate entry")) {
                        this.successMessage = false;
                        this.errorMessage = true;
                    } else {
                        console.log(error);
                    }
                })
        }
    }

}
</script>

<style scoped>
@import url('https://fonts.googleapis.com/css2?family=Montserrat:wght@200;400;500&display=swap');

h1,
h5,
p,
a {
    font-family: Montserrat, system-ui, -apple-system, BlinkMacSystemFont, 'Segoe UI', Roboto, Oxygen, Ubuntu, Cantarell, 'Open Sans', 'Helvetica Neue', sans-serif;
    font-weight: 400;
}

button {
    font-family: Montserrat, system-ui, -apple-system, BlinkMacSystemFont, 'Segoe UI', Roboto, Oxygen, Ubuntu, Cantarell, 'Open Sans', 'Helvetica Neue', sans-serif;
}

.inputStyle {
    font-family: Montserrat, system-ui, -apple-system, BlinkMacSystemFont, 'Segoe UI', Roboto, Oxygen, Ubuntu, Cantarell, 'Open Sans', 'Helvetica Neue', sans-serif;
    width: 50%;
    border-top: none;
    border-left: none;
    border-right: none;
}
</style>