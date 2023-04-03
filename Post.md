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
    -
        
