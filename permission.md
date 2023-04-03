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

## CREATE USER RESOURCE 
    - If you logout , the Dasboard link appeared
            props:Object
                auth:Object
                    user:Object
                        permissions:null
                        roles:null
    - create a user resource
            php artisan make:resource UserResource 
    - Open the  user resource aand the logic 
    - Open the HandleInertiaRequests.php file  and remove roles and permissions
    - return the created resource  new UserResource($request->user())
    - Reload the application again , you will see the Login and Register page
    - Login in again ,showinng error 
            permissions.js:4 Uncaught (in promise) TypeError: Cannot read properties of undefined (reading 'includes')
    - Solution
            . Open the AuthenticatedLayout.vue and remove v-if
                v-if="hasRole('admin')"
            . Dashbaord will be displaying
            . Vue Tool open
                    props:Object
                        auth:Object
                            user:Object
                                data:Object
                                    email:"admin@example.com"
                                    id:2
                                    name:"admin"
                                permissions:Array[0]
                                 roles:Array[1]
            . Add the data inside the composables.js
                    const hasRole       = (name) => usePage().props.auth.user.data.roles.includes(name);
                     const hasPermission = (name) => usePage().props.auth.user.data.permissions.includes(name);
            . Activate againn the  v-if="hasRole('admin')" in  AuthenticatedLayout.vue
            .Test OK
    - PROBLEM NAME ISNOT SHOWING, solution is beacuse of  user->data 
       https://laravel.com/docs/10.x/eloquent-resources#data-wrapping
        . add into app/Providers/AppServiceProvider.php
            JsonResource::withoutWrapping();
    - In permissions.js we dont need to add the data
           const hasRole       = (name) => usePage().props.auth.user.data.roles.includes(name);
           const hasPermission = (name) => usePage().props.auth.user.data.permissions.includes(name);

                TO
             const hasRole       = (name) => usePage().props.auth.user.roles.includes(name);
           const hasPermission = (name) => usePage().props.auth.user.permissions.includes(name);
    - Reload the page , the name appeared .OK
    - IF WE REGISTER AS NORMAL USER ,LINK OF ADMIN WILL BE HIDE ,OTHERWISE IF YOU LOG AS ADMIN

#### CREATE ROUTES , LINKS AND CONTROLLER 
    - if you log as admin , we need to see user , role and permission link
    - Let us create controllers
              What should the controller be named?  
        php artisan make:controller
        ❯ UserController
        
        Which type of controller would you like: [empty]
        empty ....................................................................... 0
        api ......................................................................... 1
        invokable ................................................................... 2
        resource .................................................................... 3
        singleton ................................................................... 4
   
    - Repeat the same process RoleController, PermissionController 
    - Register the route in web file 
                Route::resource('/users', UserController::class);
                Route::resource('/roles', RoleController::class);
                Route::resource('/permissions', PermissionController::class);
    - Let open the AdminLayout.vue
            . Open NavLink.vue and save as SidebarLink
            . Add the SidebarLink in  AdminLayout.vue
            . Comment/remove the other li 
            . create the sideLink for both , users, Roles, Permission
    - Open the User Controller and add logic in index method
    - Create  folder inside the pages called Users/Index.vue
    - Copy the file from AdminIndex.vue and paste to Index.vue
    - TESK ok
    -Do the same procedure to all Roles, Permissions
         NB: RESPONSE TYPE IS INERTIA
    - Open the Authenticated Layout file 
             v-if="hasRole('admin')"
             v-if="hasRole('admin')"
             :href="route('users.index')"
             :active="route().current('users.index')">
             Admin
    - Purpose of the above code in Authenticated Layout is to navigate to users page 
    - Work on Logout functionality, open AdminLayout file
            . logout button , change into Link

                      <Link
                    :href="route('logout')"
                    method="post"
                    as="button" </Link>
    - Add the color on span of logout and on svg hover:text-red-400
    - TEST APPLICATION - PASSED

### DISPLAY USERS , ROLES AND PERMISSIONS
    - Add the table to display all the users , roles and permissions
    - Visit https://flowbite.com/docs/components/tables/
    - We can't add directly on the Index page , we gonna create components
        . create Table.vue components
        . Going to create a componentt for tr, th, td
                TableRow.vue
                TableHeaderCell.vue
                TableDataCell.vue
        . Add the logic based on default Table.vue file
    - Open tthe Table.vue file and remove the tr and replace with slot
                <slot name="header"></slot>
    - In the body of Tablee.vue  remove the td and replace 
                <slot ></slot>
    - Open the User Controller inn index() and logic 
    - Open the Users/Index.vue and defineProps
            defineProps(['users'])
        . import the following Users/Index.vue
                    Table.vue
                    TableRow.vue
                    TableHeaderCell.vue
                    TableDataCell.vue
    - Loop through tto display the users
    - Repeat the same procedure to all of Roles/index.vue and Permissions/index.vue
        .copy from  Users/Index.vue and replace to Roles/Index.vue AND Permissions/index.vue
        . define the props FOR both
    - Create a Resource for Roles and Permissions
            php artisan make:resource RoleResource 
            php artisan make:resource PermissionResource   
    - Open the RoleController and return RoleResource
    - Open the PermissiionController and return RoleResource

## CREATE ROLES  
    - Object  Link to Add/Create Roles 
    - Open the Roles/index.vue page
        .Add the Link to navigate to create page
    - Open RoleController , add the logic inside the create() method , return inertia response 
    - Create a Roles/Create.vue  , copy index and paste then edit
    - Add the form , go login.vue file code the form and paste 
    - Adjust the code according to your requirement
    - Create the CreateRoleRequest
            php artisan make:request CreateRoleRequest 
    - Open the CreateRoleRequest  return the logic
    - Open the RoleController and use in store()
        . add the logic inside
             Role::create(['name' => $request->name()]);
    - Test application : Passed

##  EDIT / DELETE ROLES
    - Objectives is to be able to delete the roles.
    - Update the validation 
    - Put the button on edit / delete , open roles/index file
    - Add the  Link for Edit
    -  Create the Edit File , copy create file and save as Edit.vue
        . define props for roles
        .continue the rest of logic inn the submit methood and route
    - Open the RoleController and add logic in edit() , update() and destroy()
    - Add another link to delete the roles in index page

## CREATE PERMISSION 
    - add the button to create permission
    - Open the PermissionController  and add logic into create() method  
    - create the another file Permissions/Create.vue
    - Open the PermissionController  and add logic into update() method  
    - Create a Request
             php artisan make:request CreatePermissionRequest
    - Apply the request in store method and write the logic to create a permission
    - TEST UI : PASSED

##  EDIT / DELETE PERMISSIONS
    - Objectives is to be able to delete the permission.
    - Update the validation 
    - Put the button on edit / delete , open roles/index file
    - Add the  Link for Edit
    -  Create the Edit File , copy create file and save as Edit.vue
        . define props for permission
        .continue the rest of logic inn the submit methood and route
    - Open the PermissionController and add logic in edit() , update() and destroy()
    - Add another link to delete the roles in index page

## CREATE USERS
    - add the button to create NEW Users
    - create the another file Users/Create.vue
    - Copy from Register file and paste into Creata.vue
    - Open the UserController  and add logic into create() method  
    - Open the register Conttroller and still some codee in store()
    - Add the logic in the store 
    - TEST UI : PASSED

##  EDIT / DELETE USERS
    - Objectives is to be able to Edit/ delete the users.
    - Add the link on create.vue file to take back to the Users/indeex.vue file
    - Add the  Link for Edit  / Delete  by copying the cell from Role/Index.vue 
    - Open the UserController and add logic in edit() , update() and destroy()
    - Create the Edit page  File , copy create file and save as Edit.vue
        . define props for roles
        . continue the rest of logic inn the submit methood and route
    - Update the validation 
    - Put the button on edit / delete , open roles/index file
    - Add another link to delete the roles in index page

### ASSIGN PERMISSIONS TO ROLES 
        https://spatie.be/docs/laravel-permission/v5/basic-usage/role-permissions
    - Objective is to assign the permissions to the roles
    - When the user click on edit page on the role , the permission will be populated.
    - To show the select , assign the permission.
    - Display all permission inn the table
    - To completed the task , let search vue multiselect
        . https://vue-multiselect.js.org/
        . https://github.com/shentao/vue-multiselect
            . npm install vue-multiselect
    - Import the component into Roles/Edit.vue  file
        import Multiselect from 'vue-multiselect'
    - Import tthe syles  in Roles/Edit.vue file
        <style src="vue-multiselect/dist/vue-multiselect.min.css"></style>
    - Copy the Multiple select with search
    - :options="source" will be all the permissions
        . pass the props in Edit.vue
                 permissions:Array
        . pass an array permissions in the formm as array
    - Open the RoleController and the logic inside the edit()
        'permissions' => PermissionResource::collection(Permission::all())
    - The pass the information in the component
    - Open the CreateRoleRequest and add the permission
    - Open the update()  method in RoleController
    - TESTED - OK
    - Open the Permissions Index.vue and copy the table
    - Add the permissions relationship in RoleResource
    - Add the selected permission inn role
        onMounted(() =>{
            form.permissions = props.role?.permissions
        })

    - add mt-6 in create.vue in Role/Create.vue



## ASSIGN ROLES AND PERMISSION TO THE USERS
        https://spatie.be/docs/laravel-permission/v5/basic-usage/role-permissions
    - The objective of this lessson is to assign the permission to users
    - Open the Roles/Edit.vue file 
        . copy all of these

                import VueMultiselect from 'vue-multiselect';
                import Table from "@/Components/Table.vue";
                import TableRow from "@/Components/TableRow.vue";
                import TableHeaderCell from "@/Components/TableHeaderCell.vue";
                import TableDataCell from "@/Components/TableDataCell.vue";
                import {onMounted, watch} from "vue";
        . open the edit.vue page in admin/users
                import the above to here
                import the stles here
                add the component VueMultiselect
        . define the props roles and permission in Users/Edit.vue
        . add inn the form the roles and peermissions as empty array
    - Add the roles & permissions  props in the edit() in UserController
    - TEST  OK
    - Add the validations 
    - Create shared user ressource
        php artisan make:resource UserSharedResource
    - Open the HandleInertiaRequest.php ,use UserSharedResourc instead of UseRRESOURCE
    - Open the UserResource and make adjustment
    - Open the Users/Edit page add the unmounted() and watch()

    - Open UserController change to return back() instead of route_to() in update() mmethod
    - Let us use the Mult-Select into Users/Create.vue  and styles
        . define props for roles and permission
        . add roles :[] and permissions:[] in tthe form
    - Open tthe UserController and copy this from edit() and paste into create()
                'roles' => RoleResource::collection(Role::all()),
                'permissions' => PermissionResource::collection(Permission::all())
            . TEST APPLICATION BY CLICK NEW  - PASSES
            . continue into store() assign the User with variable $user
            . Add the roles and permission in store 
            . TEST APPLICATION BY CLICK NEW  - PASSES
    -  To revoke the Roles and Permissions by creating two controllers
        php artisan make:controller  RemoveRoleFromUserController
        php artisan make:controller  RevokePermissionFromUserController
    - Add two web route
    - Add the web route name on Users/Edit.vue  on revoke 
    - Add the funtionality in both Controllers to revoke
    - TEST THE APPLICATION - PASSED
    - NOTE
        . We need to update the Roles and Permissions once u delete will not be shown
        . open the Users/Edit.vue file  and add the logic on watch()
        . we can add the preserve-scroll on links
            https://inertiajs.com/links#scroll-preservation
            



