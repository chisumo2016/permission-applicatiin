## CREATE POST MODEL CONTROLLER AND INDEX PAGE  
    - Objective is create post page in order to user the Permissions
    - php artisan make:model -mc  (Post)
    - Open the web route and add the url for posts
    - Open AuthenticatedLayout.php and add the Link  to navigate to post page
    - Open the PostController  and add index() method 
    - Create a PostResource
         php artisan make:resource PostResource  
    - Create the Admin/Posts/Index.vue 
        . copy roles folder and paste as posts
        . edit the props and method
        . php artisan migrate:fresh --seed

    - Add the Post Link to AdminLayout.vue and icon https://heroicons.com/ - post
    - Idea behind is 
            . To generate the post policy
            . To protect
                    who can create , 
                    who can edit , 
                    who can delete , 
                    who can see the post
    - The idea is to use the roles and permissions

##  CREATE  POST CRUD 
    - Objective is to create the post 
    - Open the PostController , add create() method  
    - Open the Create.vue file and edit to reflect the input
    - Create another method to store() the information
    - Create a request called CreatePostRequest and logic
             php artisan make:request CreatePostRequest 
    - Add the mass assigment in post
    - Add the edit() and return the Inertia with PostResource
    - Open the Posts/Edit.vue add the props  post
    - Open the Posts/Index.vue and add the post inn edit
    - Add tthe update() and destroy() 
    - Pass the props in the index.vue of the post



        
