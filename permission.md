 ## PERMISSION
    - Use Inertia and Vue Js
        https://laravel.com/docs/10.x/starter-kits#breeze-and-inertia
        laravel new inertia-permission --breeze

## SPATIE 
    - install  spatie laravel permission
        https://spatie.be/docs/laravel-permission/v5/introduction
        composer require spatie/laravel-permission
        php artisan vendor:publish --provider="Spatie\Permission\PermissionServiceProvider"
        php artisan optimize:clear
        php artisan migrate
## CREATE ROLE AND ADMIN SEEDER 
    - Create a RoleSeeder & AdminSeeder
            php artisan make:seeder RoleSeeder
            php artisan make:seeder AdminSeeder
    - Add logic to both seeders
    - Call all seeders in Database Seeeder
    - Seed into database

## CONTROLLER
    - Make admin controller
        php artisan make:controller AdminController
    - Add the logic inside controller
    - Add the admin routes
    - Create the AdminIndex pages, copy from dashboard and save as resources/js/Pages/Admin/AdminIndex.vue
    - You can access the admin page via 
            http://permission-application.test/admin
    -

## TAILWINDCSS ADMIN LAYOUT  
    - Create admin panel 
    - Add the link for Admin resources/js/Layouts/AuthenticatedLayout.vue
    -  Copy the Layout from Tailwindcss components
            https://tailwindcomponents.com/components/layout
    - Save the AuthentiocatedLayout.vue as AdminLayout.vue
    - Paste the code  https://tailwindcomponents.com/components/layout
    - Then in AdminIndex.Vue extends to AdminLayou.vue
    -  Remove svg inside AdminLayout and add <slot></slot> in container
    - We want normal user not to be able too access the admin panel
            https://spatie.be/docs/laravel-permission/v5/basic-usage/middleware
        . Add to $middlewareAliases  inside the karnel
            'role' => \Spatie\Permission\Middlewares\RoleMiddleware::class,
    example
            Route::group(['auth', middleware' => ['role:name of the role']], function () {
                //
            });
    - Try to log as user  403 USER DOES NOT HAVE THE RIGHT ROLES.
    - Try to log as admin  access the dashboard

## SHARE ROLES AND PERMISSION
    - The objectives is to hide the Admin  link if ur normal user.
    - use the https://inertiajs.com/shared-data
            'auth.user' => fn () => $request->user()
                ? $request->user()->only('id', 'name', 'email')
                : null,
    - Open the HandleInertiaRequests
    - Before we code , open the devtool vue
        prpos ->auth -> user -> return all details
    - Take this code from doc   replace
        'auth.user' => fn () => $request->user()
                ? $request->user()->only('id', 'name', 'email')
                : null,
    - reresh the pages
            props:Object
                auth:Object
                user:Object
                email:"admin@example.com"
                id:2
                name:"admin"
    - Add the user roles and permission in HandleInertiaRequest
            https://spatie.be/docs/laravel-permission/v5/basic-usage/basic-usage
            /**Roles*/
            'auth.user.roles' => fn () => $request->user()
                ? $request->user()->getRoleNames()
                : null,
            /**permission*/
            'auth.user.permissions' => fn () => $request->user()
                ? $request->user()->getPermissionNames()
                : null,
    - Reload the page , check the Vue DevTools
                props:Object
                    auth:Object
                        user:Object
                        email:"admin@example.com"
                        id:2
                        name:"admin"
                        permissions:Array[0]
                        roles:Array[1]
                                 0:"admin"

    - Accesss the shared data  via usePage()
    - Open the AuthenticatedLayout.vue and add the roles as admin
            v-if="$page.props.auth.user.roles.includes('admin')"
    - Create Composables folder inside js, add new file called permissions.js
    - create a logic inside the permissions.js file
    - We can use inside thhe AuthenticatedLayout.vue  and import
            import {usePermission  } from "@/Composables/permissions"
    - Destruct the hasRole  in AuthenticatedLayout.vue 
            const { hasRole} = usePermission();
    - We can use on navlink
         v-if="$page.props.auth.user.roles.includes('admin')"
        TO
         v-if="hasRole('admin')"
    - Try again to log as normal user and admin
    - If you logout , the Dasboard link appeared
