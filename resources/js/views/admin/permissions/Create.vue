<template>
    <div class="row justify-content-center my-5">
        <div class="col-md-6">
            <div class="card border-0 shadow-sm">
                <div class="card-body">
                    <form @submit.prevent="submitForm">
                        <!-- Title -->
                        <div class="mb-3">
                            <label for="post-name" class="form-label">
                                Title
                            </label>
                            <input v-model="permission.name" id="post-name" type="text" class="form-control">
                            <div class="text-danger mt-1">
                                {{ errors.name }}
                            </div>
                            <div class="text-danger mt-1">
                                <div v-for="message in validationErrors?.name">
                                    {{ message }}
                                </div>
                            </div>
                        </div>
                        <!-- Buttons -->
                        <div class="mt-4">
                            <button :disabled="isLoading" class="btn btn-primary" :style="{ backgroundColor: 'red', color: 'white', borderColor: 'red' }">
                                <div v-show="isLoading" class=""></div>
                                <span v-if="isLoading">Processing...</span>
                                <span v-else>Save</span>
                            </button>
                        </div>
                    </form>
                </div>
            </div>
        </div>
    </div>
</template>
<script setup>
    import { reactive } from "vue";
    import usePermissions from "@/composables/permissions";
    import { useForm, useField, defineRule } from "vee-validate";
    import { required, min } from "@/validation/rules"
    defineRule('required', required)
    defineRule('min', min);

    // Define a validation schema
    const schema = {
        name: 'required|min:3'
    }
    // Create a form context with the validation schema
    const { validate, errors } = useForm({ validationSchema: schema });
    // Define actual fields for validation
    const { value: name } = useField('name', null, { initialValue: '' });
    const { storePermission, validationErrors, isLoading } = usePermissions();
    const permission = reactive({
        name
    })
    function submitForm() {
        validate().then(form => { if (form.valid) storePermission(permission) })
    }
</script>
