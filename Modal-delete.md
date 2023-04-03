## ADD MODAL CONFIRMATION ON DELETE USER
    - Objecttive is to add the confrimation when deleting the user
    - Open the Users/Index.vue page , navigate onn Link to delete
        . remmove this link
                <Link :href="route('users.destroy', user.id)" method="DELETE" as="button" class="text-red-400 hover:text-red-600">Delete</Link>
        . Add the button instead .
        . Add the ref()
        . Add the showConfirmDeleteUserModel() method
        . Import the Modal , DangerButton , SecondaryButton  from components into Index.vue
        . Add the method to delete user

## CONFIRMATION DELETE MODAL FOR ROLES AND PERMISSIONS
    - Objective is to add the Modal for roles and permissions 
    -  Copy the componentt form user  and paste into Roles/Index.vue
        import  Modal from  "@/Components/Modal.vue";
        import  DangerButton from  "@/Components/DangerButton.vue";
        import  SecondaryButton from  "@/Components/SecondaryButton.vue";
