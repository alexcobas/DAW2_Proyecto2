<template>

	<div class="grid">
		<div class="col-12 md:col-4 lg:col-4 xl:col-3">
		</div>

		<div class="col-12 card">
			<div class="row">
				<div class="col-12 sm:col-6 xl:col-3">
					<FileUpload name="avatar" accept="image/*" :maxFileSize="1500000" @select="onSelectedAvatar"
						pt:content:class="fu-content" pt:buttonbar:class="fu-header" pt:root:class="fu" class="fu">

						<template #header="{ chooseCallback, clearCallback, files }">
							<div class="flex flex-wrap justify-content-between align-items-center flex-1 gap-2">
								<div class="flex gap-2">
									<Button @click="chooseCallback()" icon="pi pi-images" rounded outlined></Button>
									<Button @click="clearCallback()" icon="pi pi-times" rounded outlined
										severity="danger" :disabled="!files || files.length === 0"></Button>
								</div>
								<p>Drag and drop files to here to upload.</p>
							</div>
						</template>

						<template #content="{ files, uploadedFiles, removeUploadedFileCallback, removeFileCallback }">
							<img v-if="files.length > 0" v-for="(file, index) of files"
								:key="file.name + file.type + file.size" role="presentation" :alt="file.name"
								:src="file.objectURL" class="object-fit-cover w-100 h-100 img-profile" />
							<div v-else>
								<img v-if="uploadedFiles.length > 0"
									:key="uploadedFiles[uploadedFiles.length - 1].name + uploadedFiles[uploadedFiles.length - 1].type + uploadedFiles[uploadedFiles.length - 1].size"
									role="presentation" :alt="uploadedFiles[uploadedFiles.length - 1].name"
									:src="uploadedFiles[uploadedFiles.length - 1].objectURL"
									class="object-fit-cover w-100 h-100 img-profile" />
							</div>
						</template>

						<template #empty>
							<img v-if="user.avatar" :src=user.avatar alt="Avatar"
								class="object-fit-cover w-100 h-100 img-profile">
							<img v-if="!user.avatar" src="/images/EmptyAvatar.webp" alt="Avatar Default"
								class="object-fit-cover w-100 h-100 img-profile">
						</template>
					</FileUpload>
				</div>
				<div class="col-12 sm:col-6 xl:col-9">
					<legend>User details</legend>
					<div class="row">
						<div class="form-group col-6 xl:col-3">
							<label for="name">Name</label>
							<input v-model="user.name" type="text" class="form-control" id="name">
							<!-- <div class="text-danger mt-1">{{ errors.name }}</div> -->
							<div class="text-danger mt-1">
								<div v-if="validationErrors.name">
									{{ validationErrors.name }}
								</div>
							</div>
						</div>
						<div class="form-group col-6 xl:col-3">
							<label for="surname1">First surname</label>
							<input v-model="user.surname1" type="text" class="form-control" id="surname1">
							<!-- <div class="text-danger mt-1">{{ errors.surname1 }}</div> -->
							<div class="text-danger mt-1">
								<div v-if="validationErrors.surname1">
									{{ validationErrors.surname1 }}
								</div>
							</div>
						</div>
						<div class="form-group col-6 xl:col-3">
							<label for="surname2">Last surname (optional)</label>
							<input v-model="user.surname2" type="text" class="form-control" id="surname2">
							<!-- <div class="text-danger mt-1">{{ errors.surname2 }}</div> -->
							<div class="text-danger mt-1">
								<div v-if="validationErrors.surname2">
									{{ validationErrors.surname2 }}
								</div>
							</div>
						</div>
						<div class="form-group col-6 xl:col-3">
							<label for="country">Country</label>
							<select v-model="user.country" class="form-select" id="country">
								<option v-for="country in countries" :key="country.code" :value="country.code">{{
									country.name }}
								</option>
							</select>
							<div class="text-danger mt-1">
								<div v-if="validationErrors.country">
									{{ validationErrors.country }}
								</div>
							</div>
						</div>
					</div>
					<div class="row">
						<div class="form-group col-6 xl:col-3">
							<label for="dni">Identity Card Number</label>
							<input v-model="user.dni" type="text" class="form-control" id="dni">
							<!-- <div class="text-danger mt-1">{{ errors.dni }}</div> -->
							<div class="text-danger mt-1">
								<div v-if="validationErrors.dni">
									{{ validationErrors.dni }}
								</div>
							</div>
						</div>
						<div class="form-group col-6 xl:col-3">
							<label for="birthDate">Birth date</label>
							<input v-model="user.birthdate" type="date" class="form-control" id="birthDate">
							<!-- <div class="text-danger mt-1">{{ errors.birthdate }}</div> -->
							<div class="text-danger mt-1">
								<div v-if="validationErrors.birthdate">
									{{ validationErrors.birthdate }}
								</div>
							</div>
						</div>
						<div class="form-group col-6 xl:col-3">
							<label for="phone_number">Phone number</label>
							<input v-model="user.phone_number" type="tel" class="form-control" id="phone_number">
							<!-- <div class="text-danger mt-1">{{ errors.phone_number }}</div> -->
							<div class="text-danger mt-1">
								<div v-if="validationErrors.phone_number">
									{{ validationErrors.phone_number }}
								</div>
							</div>
						</div>
						<div class="form-group col-6 xl:col-3">
							<label for="gender">Gender</label>
							<select v-model="user.gender" class="form-select" id="gender">
								<option value="M">Male</option>
								<option value="F">Female</option>
								<option value="N">Neutral</option>
							</select>
							<!-- <div class="text-danger mt-1">{{ errors.gender }}</div> -->
							<div class="text-danger mt-1">
								<div v-if="validationErrors.gender">
									{{ validationErrors.gender }}
								</div>
							</div>
						</div>
					</div>
					<legend>Account details</legend>
					<div class="row">
						<div class="form-group col-4">
							<label for="username">Username</label>
							<input v-model="user.username" type="text" class="form-control" id="username">
							<!-- <div class="text-danger mt-1">{{ errors.username }}</div> -->
							<div class="text-danger mt-1">
								<div v-if="validationErrors.username">
									{{ validationErrors.username }}
								</div>
							</div>
						</div>
						<div class="form-group col-4">
							<label for="email">Email</label>
							<input v-model="user.email" type="email" class="form-control" id="email">
							<!-- <div class="text-danger mt-1">{{ errors.email }}</div> -->
							<div class="text-danger mt-1">
								<div v-if="validationErrors.email">
									{{ validationErrors.email }}
								</div>
							</div>
						</div>
						<div class="form-group col-4">
							<label for="gender">Role</label>
							<select v-model="user.role_id" class="form-select" id="gender">
								<option v-for="role in roleList" :key="role.id" :value="role.id">{{ role.name }}
								</option>
							</select>
						</div>
					</div>
					<div class="row d-flex align-items-center" v-if="!editMode">
						<div class="col-12 xl:col-3" :class="width < 1024 ? 'd-flex justify-content-center' : ''">
							<div class="form-check ps-1">
								<input class="form-check-input m-0" v-model="user.automaticValidation" type="checkbox"
									value="" id="autoValidate" @change="toggleAutoVerify">
								<label class="form-check-label my-0 ps-2 h-100 d-flex align-items-center"
									for="autoValidate">
									Validate this user at creation
								</label>
							</div>
						</div>
						<div class="col-12 xl:col-3" :class="width < 1024 ? 'd-flex justify-content-center' : ''"
							v-if="autoVerify">
							<FileUpload @select="onSelectValidationImage($event, 'front')" class="justify-content-start"
								mode="basic" name="validationImages[]" accept="image/*" :maxFileSize="1000000"
								chooseLabel="Front image"
								:style="{ backgroundColor: 'red', color: 'white', borderColor: 'red' }" />
						</div>
						<div class="col-12 xl:col-3" :class="width < 1024 ? 'd-flex justify-content-center' : ''"
							v-if="autoVerify">
							<FileUpload @select="onSelectValidationImage($event, 'back')" class="justify-content-start"
								mode="basic" name="validationImages[]" accept="image/*" :maxFileSize="1000000"
								chooseLabel="Back image"
								:style="{ backgroundColor: 'red', color: 'white', borderColor: 'red' }" />
						</div>
						<div class="col-12 xl:col-3" :class="width < 1024 ? 'd-flex justify-content-center' : ''"
							v-if="autoVerify">
							<FileUpload @select="onSelectValidationImage($event, 'face')" class="justify-content-start"
								mode="basic" name="validationImages[]" accept="image/*" :maxFileSize="1000000"
								chooseLabel="Face image"
								:style="{ backgroundColor: 'red', color: 'white', borderColor: 'red' }" />
						</div>
					</div>
				</div>
			</div>
		</div>
	</div>
	<div class="row d-flex justify-content-end mt-5">
		<div class="text-right col-6 md:col-4 xl:col-2">
			<button :disabled="isLoading" class="btn btn-primary w-100" @click="submitForm"
				:style="{ backgroundColor: 'red', color: 'white', borderColor: 'red' }">
				<div v-show="isLoading" class=""></div>
				<span v-if="isLoading">Processing...</span>
				<span v-else>Save user</span>
			</button>
		</div>
	</div>

	<Toast />
</template>

<script setup>
import { onMounted, reactive, ref, watchEffect } from "vue";
import * as yup from 'yup';
import { useRoute } from "vue-router";
import { usePrimeVue } from 'primevue/config';
import useRoles from "@/composables/roles";
import useUsers from "@/composables/users";
import useCountries from "@/composables/countries";
import { useToast } from 'primevue/usetoast';
import { useWindowSize } from '@vueuse/core'

const { width, height } = useWindowSize()

const $primevue = usePrimeVue();
const toast = useToast();
const { roleList, getRoleList } = useRoles();
const { countries, getCountries } = useCountries();
const { updateUser, storeUser, getUser, user: postData, createUserDB, deleteUserDB, changeUserPasswordDB, createUserProceduredDB, validationErrors, isLoading } = useUsers();

import { required, min } from "@/validation/rules"
import { FileUpload } from "primevue";
import { get } from "lodash";

const eightteenYearsAgo = new Date(new Date().setFullYear(new Date().getFullYear() - 18));

// Define a validation schema
const schema = yup.object({
	username: yup.string().required('Username is required').max(255).min(9),
	name: yup.string().required('Name is required').max(255),
	surname1: yup.string().required('First surname is required').max(255),
	surname2: yup.string().nullable().max(255),
	birthdate: yup.date()
		.required('Birthdate is required')
		.typeError('Birthdate must be a valid date')
		.max(eightteenYearsAgo, 'User must be at least 18 years old'),
	email: yup
		.string()
		.required('Email is required')
		.email('Email must be valid')
		.max(255),
	dni: yup
		.string()
		.required('DNI is required')
		.matches(/^\d{8}[A-Za-z]$/, 'DNI must have 8 digits followed by a letter')
		.max(9, 'DNI must be exactly 9 characters'),
	gender: yup.string().required('Gender is required').oneOf(['N', 'M', 'F']),
	phone_number: yup.string().required('Phone number is required').max(255),
	// password: yup
	// 	.string()
	// 	.required('Password is required')
	// 	.min(8, 'Password must be at least 8 characters'),
	// password_confirmation: yup
	// 	.string()
	// 	.oneOf([yup.ref('password'), null], 'Passwords must match'),
	country: yup.string().required('Country is required').max(255),
	terms: yup.boolean().oneOf([true], 'You must accept the terms and conditions'),
	validationImages: yup.array().of(
		yup.mixed().required()
	)
})

const user = reactive({
	name: '',
	email: '',
	surname1: '',
	surname2: '',
	country: '',
	role_id: '',
	dni: '',
	phone_number: '',
	gender: '',
	birthdate: '',
	username: '',
	automaticValidation: '',
	validationImages: [],
	avatar: []
})

const route = useRoute()
const submitForm = () => {
	validationErrors.value = {}; // Reset errors
	schema.validate(user, { abortEarly: false })
		.then(() => {
			if (props.editMode) {
				updateUser(user)
			} else {
				if (typeof user.role_id === 'object') {
					user.role_id = user.role_id[0].id; //TODO: Refactor this conversion
				}
				storeUser(user)
			}
		})
		.catch((err) => {
			err.inner.forEach((error) => {
				validationErrors.value[error.path] = error.message;
			});
		});
}

// function submitForm() {
// 	validate().then(form => {
// 		if (form.valid) {
// 			if (props.editMode) {
// 				updateUser(user)
// 			} else {
// 				if (typeof user.role_id === 'object') {
// 					user.role_id = user.role_id[0].id; //TODO: Refactor this conversion
// 				}
// 				storeUser(user)
// 			}
// 		}
// 	})
// }

const props = defineProps({
	editMode: {
		type: Boolean,
		required: false
	}
});

onMounted(() => {
	getCountries()
	getRoleList()
	if (props.editMode) {
		getUser(route.params.id)
	}
})
// https://vuejs.org/api/reactivity-core.html#watcheffect
watchEffect(() => {
	user.id = postData.value.id
	user.name = postData.value.name
	user.email = postData.value.email
	user.surname1 = postData.value.surname1
	user.surname2 = postData.value.surname2
	user.country = postData.value.country
	user.role_id = postData.value.role_id
	user.avatar = postData.value.avatar
	user.dni = postData.value.dni
	user.phone_number = postData.value.phone_number
	user.gender = postData.value.gender
	user.username = postData.value.username
	user.birthdate = postData.value.birthdate
})

const totalSize = ref(0);
const totalSizePercent = ref(0);
const files = ref([]);
const autoVerify = ref(false);

// const limitMultiImage = () => {

// };

const toggleAutoVerify = () => {
	autoVerify.value = !autoVerify.value;
}

const onSelectValidationImage = (event, type) => {
	let index;

	// Determinar el índice según el tipo de imagen
	if (type === 'front') {
		index = 0;
	} else if (type === 'back') {
		index = 1;
	} else if (type === 'face') {
		index = 2;
	} else {
		console.error('Invalid image type');
		return;
	}

	// Reemplazar o agregar la imagen en el índice correspondiente
	if (event.files && event.files[0]) {
		user.validationImages[index] = event.files[0];
		// console.log(`Validation Images:`, JSON.stringify(user.validationImages, null, 2));
	}
};

const onBeforeUpload = (event) => {
	// console.log('onBeforeUpload')
	event.formData.append('id', user.id)
};
const onRemoveTemplatingFile = (file, removeFileCallback, index) => {
	removeFileCallback(index);
	totalSize.value -= parseInt(formatSize(file.size));
	totalSizePercent.value = totalSize.value / 10;
};

const onClearTemplatingUpload = (clear) => {
	clear();
	totalSize.value = 0;
	totalSizePercent.value = 0;
};

const onSelectedAvatar = (event) => {
	user.avatar = event.files;
};

const createUserDBView = async (id) => {
	createUserDB(id).then(response => {
		toast.add({ severity: 'info', summary: 'Base de datos creada', detail: 'Base de datos creada correctamente.', life: 3000 });
	})
		.catch(error => {
			toast.add({ severity: 'warning', summary: 'Error al crear la base de datos', detail: error.response.data.message, life: 3000 });
			console.log(error.response.data.message)
		})
}

const changeUserPasswordDBView = async (id) => {
	changeUserPasswordDB(id).then(response => {
		toast.add({ severity: 'info', summary: 'Base de datos modificada', detail: 'Contraseña cambiada correctamente.', life: 3000 });
	})
		.catch(error => {
			toast.add({ severity: 'danger', summary: 'Error al cambiar la contraseña', detail: error.response.data.message, life: 3000 });
		})
}

const createUserProceduredDBView = async (id) => {
	createUserProceduredDB(id).then(response => {
		toast.add({ severity: 'info', summary: 'Base de datos creada', detail: 'Base de datos creada correctamente.', life: 3000 });
	})
		.catch(error => {
			toast.add({ severity: 'warning', summary: 'Error al crear la base de datos', detail: error.response.data.message, life: 3000 });
			console.log(error.response.data.message)
		})
}

const deleteUserDBView = async (id) => {
	deleteUserDB(id).then(response => {
		toast.add({ severity: 'info', summary: 'Base de datos creada', detail: 'Base de datos creada correctamente.', life: 3000 });
	})
		.catch(error => {
			toast.add({ severity: 'warning', summary: 'Error al crear la base de datos', detail: error.response.data.message, life: 3000 });
			console.log(error.response.data.message)
		})
}


const onTemplatedUpload = (event) => {
	// console.log('onTemplatedUpload');
	// console.log(event);
};

const formatSize = (bytes) => {
	const k = 1024;
	const dm = 3;
	const sizes = $primevue.config.locale.fileSizeTypes;

	if (bytes === 0) {
		return `0 ${sizes[0]}`;
	}

	const i = Math.floor(Math.log(bytes) / Math.log(k));
	const formattedSize = parseFloat((bytes / Math.pow(k, i)).toFixed(dm));

	return `${formattedSize} ${sizes[i]}`;
};

</script>

<style scoped>
.fu-content {
	padding: 0px !important;
	border: 0px !important;
	border-radius: 6px;
}

.fu-header {
	border: 0px !important;
	border-radius: 6px;
}

.fu {
	display: flex;
	flex-direction: column-reverse;
	border-radius: 6px;
	border: 1px solid #e2e8f0;
	margin-bottom: 10px;
}


.img-profile {
	border-top-right-radius: 6px;
	border-top-left-radius: 6px;
	aspect-ratio: 1/1;
}

.form-group {
	margin-bottom: 1rem;
}

label {
	margin-bottom: 0.3rem;
}

input[type="checkbox"] {
	width: 1.5em;
	height: 1.5em;
}

.p-fileupload-basic {
	justify-content: flex-start !important;
}

.p-fileupload-choose-button {
	background: red !important;
	border: none !important;
}
</style>