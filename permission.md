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
