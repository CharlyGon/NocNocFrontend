<template>
    <div class="max-w-md mx-auto p-6 bg-white rounded-lg shadow-md">
        <form @submit.prevent="handleSubmit">
            <div class="mb-5">
                <label for="name" class="block mb-2 text-sm font-medium text-gray-900 dark:text-black">Name</label>
                <input v-model="formData.name" type="text" id="name"
                    class="shadow-sm bg-gray-50 border border-gray-300 text-gray-900 text-sm rounded-lg focus:ring-blue-500 focus:border-blue-500 block w-full p-2.5 dark:bg-gray-700 dark:border-gray-600 dark:placeholder-gray-400 dark:text-white dark:focus:ring-blue-500 dark:focus:border-blue-500 dark:shadow-sm-light"
                    placeholder="Nombre Apellido" required />
            </div>

            <div class="mb-5">
                <label for="email" class="block mb-2 text-sm font-medium text-gray-900 dark:text-black">
                    Email</label>
                <input v-model="formData.email" type="email" id="email"
                    class="shadow-sm bg-gray-50 border border-gray-300 text-gray-900 text-sm rounded-lg focus:ring-blue-500 focus:border-blue-500 block w-full p-2.5 dark:bg-gray-700 dark:border-gray-600 dark:placeholder-gray-400 dark:text-white dark:focus:ring-blue-500 dark:focus:border-blue-500 dark:shadow-sm-light"
                    placeholder="name@ejemplo.com" required />
            </div>

            <button type="submit"
                class="text-white bg-blue-700 hover:bg-blue-800 focus:ring-4 focus:outline-none focus:ring-blue-300 font-medium rounded-lg text-sm px-5 py-2.5 text-center dark:bg-blue-600 dark:hover:bg-blue-700 dark:focus:ring-blue-800 block mx-auto">Register
                new account</button>
        </form>

        <div class="text-center mt-4">
            <p v-if="successMessage" class="text-green-500">{{ successMessage }}</p>
            <p v-if="errorMessage" class="text-red-500">{{ errorMessage }}</p>
        </div>
    </div>
</template>

<script setup>
import { ref } from 'vue';
import { registerUser } from '../api/api.js';
import { useStore } from 'vuex';

const store = useStore();
const formData = ref({
    name: '',
    email: ''
});
const successMessage = ref('');
const errorMessage = ref('');

const handleSubmit = async () => {
    try {
        const token = store.state.token;
        if (!token) {
            throw new Error('No se encontró el token de autorización.');
        }

        await registerUser(formData.value, token);

        resetFormData();
        successMessage.value = 'Usuario creado exitosamente';

        setTimeout(() => {
            successMessage.value = '';
        }, 3000);
    } catch (error) {
        handleError(error);
    }
};

const resetFormData = () => {
    formData.value.name = '';
    formData.value.email = '';
};

const handleError = (error) => {
    errorMessage.value = 'Error al crear usuario';
    console.error('Error al crear usuario:', error);
};
</script>
