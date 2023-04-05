### PROTECT POST WITH POST POLICY
        https://laravel.com/docs/10.x/authorization#main-content
    - Objectives is to use roles and permission to protect our route
    - Assume , my Role is Admin,  I would like to create, edit , delete a Post
    - Assume , my Role is Moderator,  I would like to create, update , delete a Post
    - Assume , my Role is Writer,  I would like to create a Post
    - Assume , in Permissions
                view posts
                create post
                update post
                delete post
    - The user who has permission to  create-post can create a post
    - The user who has permission to  update-post can update a post
    - Users
            .admin  -> Role is Admin
            .writer -> Role is Writer
            .Moderator -> Role is Moderator
            .User  -> Role is User
    - I  need to protect the Post with policy
    - Create a policy
        php artisan make:policy PostPolicy --model=Post
        . Create Post  -> Role of admin and moderator
        https://laravel.com/docs/10.x/authorization#via-controller-helpers
    - Write  logic to inside the policy
            eg admin and moderator 
                .log as admin -> PASSED
                . log as moderator ->
         return $user->hasRole(['admin', 'moderator','writer']) ? true :false;
    - We can use the spatie library as well  hasPermissionTo
        return $user->hasPermissionTo('create post') ? true :false;

## PROTECT ROUTES WITH ROLES 
        https://spatie.be/docs/laravel-permission/v5/basic-usage/middleware
    - Objective is to protect the routes
    - If u login as Moderator
        . want to visit the  , post , permission , role and users
    - If u login as Writer
        . visit the  , post 
    - Open the kernel php
            app/Http/Kernel.php
    - Open the web
        . Cut all the roles.users,permissionn and put into auth middlware
        . add thhe prefix('/admin')

    - To hide and show the post post based on role user has
        . Open the authenticated Layout
        .add the followinng method in permissions.js
            const hasRoles      = (names) => usePage().props.auth.user.roles.some(name => names.includes(name));
        . Use it in Authenticated Layaout
                <NavLink
                    v-if="hasRoles(['admin','moderator','writer'])"
                    :href="route('posts.index')"
                    :active="route().current('posts.index')">
                    Posts
                </NavLink>
    - Example if you want to hide the users, roles and permissions
        . navigate into AdminLayout.vue
            import {usePermission  } from "@/Composables/permissions"

        . Add on the links
            <template v-if="hasRole('admin')">
                
            </template>

    - Try to log  as admin 
    - TESTED = PASS
        
