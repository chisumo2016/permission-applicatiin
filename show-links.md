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
