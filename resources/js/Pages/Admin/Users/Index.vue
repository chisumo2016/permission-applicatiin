
<template>
    <Head title="Users Index" />

    <AdminLayout>
        <template #header>
            <h2 class="font-semibold text-xl text-gray-800 leading-tight">Admin</h2>
        </template>

        <div class="max-w-7xl  mx-auto py-4">
            <div class="flex justify-between">
                <h1>Users Index  </h1>
                <Link :href="route('users.create')" class="px-3 py-2 text-white font-semibold bg-indigo-500 hover:bg-indigo-700 rounded">New User</Link>
            </div>
            <div class="mt-6">
                <Table>
                    <template #header>
                        <TableRow>
                            <TableHeaderCell>ID</TableHeaderCell>
                            <TableHeaderCell>Name</TableHeaderCell>
                            <TableHeaderCell>Email</TableHeaderCell>
                            <TableHeaderCell>Action</TableHeaderCell>
                        </TableRow>
                    </template>
                    <template #default>
                        <TableRow v-for="user in users" :key="user.id" class="border-b">
                            <TableDataCell>{{ user.id}}</TableDataCell>
                            <TableDataCell>{{ user.name }}</TableDataCell>
                            <TableDataCell>{{ user.email }}</TableDataCell>
                            <TableDataCell class="space-x-4">
                                <Link :href="route('users.edit',    user.id)" class="text-green-400 hover:text-green-600">Edit</Link>
                                <button class="text-red-400 hover:text-red-600" @click="confirmDeleteUser">Delete</button>
                                <Modal :show="showConfirmDeleteUserModel" @close="closeModal">
                                    <div class="p-6">
                                        <h2 class="text-lg font-semibold text-slate-800">Are you sure you want to delete this user ?</h2>
                                        <div class="mt-6 flex space-x-4">
                                            <DangerButton @click="$event => deleteUser(user.id)">Delete</DangerButton>
                                            <secondary-button @click="closeModal">Cancel</secondary-button>
                                        </div>
                                    </div>
                                </Modal>
<!--                                <Link :href="route('users.destroy', user.id)" method="DELETE" as="button" class="text-red-400 hover:text-red-600">Delete</Link>-->
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


defineProps(['users']);
const form = useForm({});

const showConfirmDeleteUserModel = ref(false);

const confirmDeleteUser = () =>{
    showConfirmDeleteUserModel.value = true
}

const  closeModal = () =>{
    showConfirmDeleteUserModel.value = false
}

const deleteUser = (id)=> {
    form.delete(route('users.destroy', id),{
        onSuccess: () => closeModal()
    })
}
</script>

