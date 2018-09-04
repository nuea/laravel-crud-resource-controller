## Laravel CRUD Resource Controller

### 1.Create new Laravel Project

    composer create-project --prefer-dist laravel/laravel crud-resource-controller

And wait until install all package is finished.
After finished, open project with Text editor.

### 2. Database Configurations
    
Create new database and setup the project.

Database is name "laravel_crud_resource_controller" 

Open ".env" file. Change confi database.

    DB_DATABASE=homestead       ==>     DB_DATABASE=laravel_crud_resource_controller

    DB_USERNAME=homestead       ==>     DB_USERNAME=root

    DB_PASSWORD=secret          ==>     DB_PASSWORD=

### 3. Back to CMD or Terminal, go to your project folder.
    cd crud-resource-controller

### 4. Start a development server at http://localhost:8000 in your project.
    php artisan serve

### 5. Create Post Migration table
    php artisan make:migration create_posts_table

### 6. Open "create_posts_table" file created by Migration.
    
Add new field in Post migration table.

    Schema::create('posts', function (Blueprint $table) {
        $table->increments('id');
        $table->string('title');
        $table->text('body');
        $table->timestamps();
    });

### 7. Open "AppServiceProvider.php" file in "app/Providers" folder.

Import Schema: 

    use Illuminate\Support\Facades\Schema;

In boot function add: 

    Schema::defaultStringLength(191);

### 8. Open "database.php" file in "config" folder.
    
Change 'mysql' setting
    
    'charset' => 'utf8mb4',                 ==>     'charset' => 'utf8',
    'collation' => 'utf8mb4_unicode_ci',    ==>     'collation' => 'utf8_unicode_ci',
    
### 9. Next to run migration command
    php artisan migrate

### 10. Now, we have new "posts" table in your database.

### 11. Create new Model and new Controller
Create 'Post' model

    php artisan make:model Post

Create 'PostController' with "--resource" parameter
    
    php artisan make:controller PostController --resource

### 12 Open Post Model => "Post.php" file in "app" folder.

Declare our table  this:

    protected $table = 'posts';
    protected $fillable = ['title','body'];

### 13. Open "PostController.php" file.

In file, use our model "Post model".

    use App\Post;

"index" method will show all data from datavase to our index view.
    
    $posts = Post::latest()->paginate(5);

    return view('posts.index', compact('posts'))->with('i',(request()->input('page',1) -1) *5);

### 14. Next, install Laravel Collective (Html and Form)
    
    composer require laravelcollective/html

### 15. After successfully install package, open "config/app.php" file and add service provider and alias.
'providers' add this:
    
    Collective\Html\HtmlServiceProvider::class,
'aliases' add this:

    'Form' => Collective\Html\FormFacade::class,
    'Html' => Collective\Html\HtmlFacade::class,

### 16. Create new Route in "web.php" file in "routes" folder.
    
    Route::resource('posts','PostController');

### 17. In the views(resources/views), create new "posts" folder and in folder create more file.
(master.blade.php, index.blade.php, edit.blade.php, show.blade.php, create.blade.php, form.blade.php)

### 18. Create "master.blade.php" : master is blade template for mastering out bootstrap pages.

### 19. Create "index.blade.php" : Now, in the index page, will show all data and store to the table.

### 20. Add new data into database and see it from our index page (http://localhost:8000/posts)

### 21. Back to "PostController.php" file go to "create" method. Create new view (create.blade.php)
    
    return view('posts.create');

### 22. Create "create.blade.php" and "form.blade.php" file and add code.

### 23. Back to "PostController.php" file for save data into database, create new method in Store function
    
    request()->validate([
            'title' => 'required',
            'body' => 'required',
    ]);

    Post::create($request->all());

    return redirect()->route('posts.index')->with('success','Post created successfully');

### 24. Test save new data into database using "store" method. (http://localhost:8000/posts/create)

### 25. Back to "PostController.php" file go to "show" method. Show method will display the single data in show page. (show.blade.php)

### 26. Create "show.blade.php" file is new page and add code. 

### 27. Back to "PostController.php" file. The "edit" method will return to the view(edit.blade.php) and display single data.
    
    $post = Post::find($id);
    
    return view('posts.edit', compact('post'));

### 28. So, create new view (edit.blade.php) and add code. 

### 29. Next, "update" method will store data into database in "PostController.php" file.
    
    request()->validate([
        'title' => 'required',
        'body' => 'required',
    ]);
    Post::find($id)->update($request->all());
    return redirect()->route('posts.index')->with('success','Post updated successfully');

### 30. The latest step, "destory" method will database selected data from database in "PostController.php" file.
    
    Post::find($id)->delete();
    return redirect()->route('posts.index')->with('success','Post deleted successfully');
