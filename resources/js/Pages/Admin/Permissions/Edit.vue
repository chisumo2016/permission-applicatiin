
<template>
    <Head title="Update Permission" />

    <AdminLayout>
       <template #header>
        <h2 class="font-semibold text-xl text-gray-800 leading-tight">Admin</h2>
    </template>

        <div class="max-w-7xl  mx-auto py-4  bg-slate-100 shadow-lg rounded-lg p-6">
           <div class="flex justify-between">
               <Link :href="route('permissions.index')" class="px-3 py-2 text-white font-semibold bg-indigo-500 hover:bg-indigo-700 rounded">Back</Link>
           </div>
            <div class="mt-6 max-w-md mx-auto">
                <h1 class="text-2xl">Update Permission</h1>
                <form @submit.prevent="submit">
                    <div>
                        <InputLabel for="name" value="Name" />

                        <TextInput
                            id="name"
                            type="text"
                            class="mt-1 block w-full"
                            v-model="form.name"
                            autofocus
                            autocomplete="name"
                        />

                        <InputError class="mt-2" :message="form.errors.name" />
                    </div>


                    <div class="flex items-center mt-4">

                        <PrimaryButton class="ml-4" :class="{ 'opacity-25': form.processing }" :disabled="form.processing">
                            Update
                        </PrimaryButton>
                    </div>
                </form>
            </div>
        </div>
    </AdminLayout>
</template>

<script setup>
import { Head ,Link , useForm} from '@inertiajs/vue3';
import AdminLayout from "@/Layouts/AdminLayout.vue";


import InputError from '@/Components/InputError.vue';
import InputLabel from '@/Components/InputLabel.vue';
import PrimaryButton from '@/Components/PrimaryButton.vue';
import TextInput from '@/Components/TextInput.vue';

const  props = defineProps({
    permission:{
        type: Object,
        required: true
    }
})
const form = useForm({
    name : props.permission.name
})

const submit =() =>{
    form.put(route('permissions.update', props.permission.id))
}
</script>

