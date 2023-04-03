
<template>
    <Head title="Posts Index" />

    <AdminLayout>
       <template #header>
        <h2 class="font-semibold text-xl text-gray-800 leading-tight">Posts</h2>
    </template>

        <div class="max-w-7xl  mx-auto py-4">
           <div class="flex justify-between">
               <h1>Posts Index  </h1>
               <Link :href="route('posts.create')" class="px-3 py-2 text-white font-semibold bg-indigo-500 hover:bg-indigo-700 rounded">Create New Post</Link>
           </div>
            <div class="mt-6">
                <Table>
                    <template #header>
                        <TableRow>
                            <TableHeaderCell>ID</TableHeaderCell>
                            <TableHeaderCell>Title</TableHeaderCell>
                            <TableHeaderCell>Action</TableHeaderCell>
                        </TableRow>
                    </template>
                    <template #default>
                        <TableRow v-for="post in posts" :key="role.id" class="border-b">
                            <TableDataCell>{{ post.id}}</TableDataCell>
                            <TableDataCell>{{ post.name }}</TableDataCell>
                            <TableDataCell class="space-x-4">
                                <Link :href="route('posts.edit',  post.id)" class="text-green-400 hover:text-green-600">Edit</Link>
                                <button class="text-red-400 hover:text-red-600" @click="confirmDeletePost">Delete</button>
                                <Modal :show="showConfirmDeletePostModel" @close="closeModal">
                                    <div class="p-6">
                                        <h2 class="text-lg font-semibold text-slate-800">Are you sure you want to delete this Post ?</h2>
                                        <div class="mt-6 flex space-x-4">
                                            <DangerButton @click="$event => deletePost(post.id)">Delete</DangerButton>
                                            <secondary-button @click="closeModal">Cancel</secondary-button>
                                        </div>
                                    </div>
                                </Modal>
<!--                                <Link :href="route('posts.destroy', post.id)" method="DELETE" as="button" class="text-red-400 hover:text-red-600">Delete</Link>-->
                            </TableDataCell>
                        </TableRow>
                    </template>
                </Table>
            </div>
        </div>
    </AdminLayout>
</template>

<script setup>
import {Head, Link, useForm} from '@inertiajs/vue3';
import AdminLayout from "@/Layouts/AdminLayout.vue";
import {ref} from "vue";
import Table from "@/Components/Table.vue";
import TableRow from "@/Components/TableRow.vue";
import TableHeaderCell from "@/Components/TableHeaderCell.vue";
import TableDataCell from "@/Components/TableDataCell.vue";

import  Modal from  "@/Components/Modal.vue";
import  DangerButton from  "@/Components/DangerButton.vue";
import  SecondaryButton from  "@/Components/SecondaryButton.vue";


defineProps(["roles"]);

const form = useForm({});

const showConfirmDeletePostModel = ref(false);

const confirmDeletePost = () =>{
    showConfirmDeletePostModel.value = true
}

const  closeModal = () =>{
    showConfirmDeletePostModel.value = false
}

const deletePost = (id)=> {
    form.delete(route('posts.destroy', id),{
        onSuccess: () => closeModal()
    })
}
</script>

