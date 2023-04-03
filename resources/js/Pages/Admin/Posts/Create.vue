
<template>
    <Head title="Create New Post" />

    <AdminLayout>
       <template #header>
        <h2 class="font-semibold text-xl text-gray-800 leading-tight">Posts</h2>
    </template>

        <div class="max-w-7xl  mx-auto py-4  bg-slate-100 shadow-lg rounded-lg p-6">
           <div class="flex justify-between">
               <Link :href="route('posts.index')" class="px-3 py-2 text-white font-semibold bg-indigo-500 hover:bg-indigo-700 rounded">Back</Link>
           </div>
            <div class="mt-6 max-w-6xl mx-auto">
                <h1 class="text-2xl font-semibold text-indigo-700">Create New Post</h1>
                <form @submit.prevent="submit">
                    <div class="mt-4">
                        <InputLabel for="title" value="Title" />

                        <TextInput
                            id="title"
                            type="text"
                            class="mt-1 block w-full"
                            v-model="form.title"
                            autofocus
                            autocomplete="title"
                        />

                        <InputError class="mt-2" :message="form.errors.title" />
                    </div>

                    <div class="flex items-center mt-4">

                        <PrimaryButton class="ml-4" :class="{ 'opacity-25': form.processing }" :disabled="form.processing">
                            Create Post
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

const form = useForm({
    title : "",
})

const submit =() =>{
    form.post(route('posts.store'))
}
</script>

