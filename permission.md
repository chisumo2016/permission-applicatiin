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

