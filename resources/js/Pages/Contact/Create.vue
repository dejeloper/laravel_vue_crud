<script setup>
import FileInput from "@/Components/FileInput.vue";
import InputError from "@/Components/InputError.vue";
import InputLabel from "@/Components/InputLabel.vue";
import PrimaryButton from "@/Components/PrimaryButton.vue";
import TextInput from "@/Components/TextInput.vue";
import AuthenticatedLayout from "@/Layouts/AuthenticatedLayout.vue";
import { Head, Link, useForm } from "@inertiajs/vue3";

const initialValue = {
    name: "",
    phone: "",
    avatar: null,
    privacity: "private",
};

const form = useForm(initialValue);

const onSelectAvatar = (event) => {
    const files = event.target.files;
    if (files.length > 0) {
        form.avatar = files[0];
    }
    console.log(form.avatar);
};

const submit = () => {
    form.post(route("contact.store"), {
        onFinish: () => form.reset("password", "password_confirmation"),
    });
};
</script>

<template>
    <Head title="Contactos" />

    <AuthenticatedLayout>
        <template #header>
            <div class="flex justify-between items-center">
                <h2 class="font-semibold text-xl text-gray-800 leading-tight">
                    Crear Contacto
                </h2>
                <Link
                    :href="route('contact.index')"
                    class="text-sm text-gray-700"
                    >Lista de Contactos
                </Link>
            </div>
        </template>

        <div class="py-12">
            <div class="max-w-7xl mx-auto sm:px-6 lg:px-8">
                <div
                    class="flex justify-center bg-white overflow-hidden shadow-sm sm:rounded-lg"
                >
                    <form class="p-6 w-1/3 space-y-3" @submit.prevent="submit">
                        <div>
                            <InputLabel for="name" value="Nombre" />

                            <TextInput
                                id="name"
                                type="text"
                                class="mt-1 block w-full"
                                v-model="form.name"
                                autocomplete="name"
                                placeholder="Jhonatan Guerrero"
                            />

                            <InputError
                                class="mt-2"
                                :message="form.errors.name"
                            />
                        </div>
                        <div>
                            <InputLabel for="phone" value="Teléfono" />

                            <TextInput
                                id="phone"
                                type="text"
                                class="mt-1 block w-full"
                                v-model="form.phone"
                                autocomplete="phone"
                                placeholder="+57 999999999"
                            />

                            <InputError
                                class="mt-2"
                                :message="form.errors.phone"
                            />
                        </div>
                        <div>
                            <InputLabel for="avatar" value="Avatar" />

                            <FileInput
                                id="avatar"
                                class="mt-1 block w-full"
                                v-model="form.avatar"
                                @change="onSelectAvatar"
                            />

                            <InputError
                                class="mt-2"
                                :message="form.errors.avatar"
                            />
                        </div>
                        <div>
                            <InputLabel for="privacity" value="Privacidad" />

                            <select
                                v-model="form.privacity"
                                id="privacity"
                                class="mt-1 block w-full border-gray-300 focus:border-indigo-500 focus:ring-indigo-500 rounded-md shadow-sm"
                            >
                                <option value="private">Privado</option>
                                <option value="public">Publico</option>
                            </select>

                            <InputError
                                class="mt-2"
                                :message="form.errors.privacity"
                            />
                        </div>
                        <div class="flex items-center justify-center">
                            <PrimaryButton
                                class="mt-6 text-center"
                                :class="{ 'opacity-25': form.processing }"
                                :disabled="form.processing"
                            >
                                Crear Contacto
                            </PrimaryButton>
                        </div>
                    </form>
                </div>
            </div>
        </div>
    </AuthenticatedLayout>
</template>
