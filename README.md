# PHP Laravel 8.83.12 CRUD Web App with MySQL

<p align="center"><a href="https://laravel.com" target="_blank"><img src="https://raw.githubusercontent.com/laravel/art/master/logo-lockup/5%20SVG/2%20CMYK/1%20Full%20Color/laravel-logolockup-cmyk-red.svg" width="400"></a></p>

<p align="center">
<a href="https://travis-ci.org/laravel/framework"><img src="https://travis-ci.org/laravel/framework.svg" alt="Build Status"></a>
<a href="https://packagist.org/packages/laravel/framework"><img src="https://img.shields.io/packagist/dt/laravel/framework" alt="Total Downloads"></a>
<a href="https://packagist.org/packages/laravel/framework"><img src="https://img.shields.io/packagist/v/laravel/framework" alt="Latest Stable Version"></a>
<a href="https://packagist.org/packages/laravel/framework"><img src="https://img.shields.io/packagist/l/laravel/framework" alt="License"></a>
</p>


## About Laravel

Laravel is a web application framework with expressive, elegant syntax. We believe development must be an enjoyable and creative experience to be truly fulfilling. Laravel takes the pain out of development by easing common tasks used in many web projects, such as:

- [Simple, fast routing engine](https://laravel.com/docs/routing).
- [Powerful dependency injection container](https://laravel.com/docs/container).
- Multiple back-ends for [session](https://laravel.com/docs/session) and [cache](https://laravel.com/docs/cache) storage.
- Expressive, intuitive [database ORM](https://laravel.com/docs/eloquent).
- Database agnostic [schema migrations](https://laravel.com/docs/migrations).
- [Robust background job processing](https://laravel.com/docs/queues).
- [Real-time event broadcasting](https://laravel.com/docs/broadcasting).

Laravel is accessible, powerful, and provides tools required for large, robust applications.


## License

The Laravel framework is open-sourced software licensed under the [MIT license](https://opensource.org/licenses/MIT).

## The Laravel App Routes

- Create Student: http://127.0.0.1:8000/students/create
- Students List: http://127.0.0.1:8000/students/

![](/img/Summary.png)



## Creating a Laravel Project

```ps
composer create-project laravel/laravel --prefer-dist laravel-crud-app
```

get inside the project ``cd laravel-crud-app``


Verify the installed Laravel app version

```ps
php artisan -V
Laravel Framework 8.83.12
```

Try to run

```ps
php artisan serve
```

## Setup MySQL Database
- Servername : ```laravel-mysql-server.mysql.database.azure.com```
- DB : ```laravel_db```
- Username : ```mysqlroot```
- Password : ```P@ssw0rd!@#```

![](/img/azdb.png)

on the project contain ```.env``` files

Open the .env file and place the folloing code

```ini
DB_CONNECTION=mysql
DB_HOST=laravel-mysql-server.mysql.database.azure.com
DB_PORT=3306
DB_DATABASE=laravel_db
DB_USERNAME=mysqlroot
DB_PASSWORD='P@ssw0rd!@#'
```

## Setting Up Migration and Model

we have created the database and configured the database by adding the credentials in the env file. 

we need to create a Model and Migration file to create the migrations, run following command.

```ps
php artisan make:model Student -m
```

file will be create at database/migration/xxx_create_students_table.php

inside Laravel project , go to ```database/migrations/ timestamp_create_students_table.php``` file, and add the following schema inside of it.

```php
<?php

use Illuminate\Database\Migrations\Migration;
use Illuminate\Database\Schema\Blueprint;
use Illuminate\Support\Facades\Schema;

class CreateStudentsTable extends Migration
{
    /**
     * Run the migrations.
     *
     * @return void
     */
    public function up()
    {
        Schema::create('students', function (Blueprint $table) {
            $table->bigIncrements('id');
            $table->string('name');
            $table->string('email');
            $table->string('phone');
            $table->string('password');            
            $table->timestamps();
        });
    }

    /**
     * Reverse the migrations.
     *
     * @return void
     */
    public function down()
    {
        Schema::dropIfExists('students');
    }
}
```

As you can see there are two types of functions inside the migration files

- The ```up()``` function allow creating/updating tables,columns and indexes.

- The ```down()``` function allows reversing an operation done by up method.

Next, we will add the ```$fillable``` property in the Student model, go to ```app/Models/Student.php``` file and add the given below code

```php
<?php

namespace App\Models;

use Illuminate\Database\Eloquent\Factories\HasFactory;
use Illuminate\Database\Eloquent\Model;

class Student extends Model
{
    use HasFactory;
    protected $fillable = ['name', 'email', 'phone', 'password'];
}
```

Fillable proberty is used insde the model, and it determines which data types or fields are mass-assignable in the database table.

Next, we have to run the following command to create the table in the database

```ps
php artisan migrate
```

if cannot connect the Azure for Mysql SSL can disable https://docs.microsoft.com/en-us/azure/mysql/flexible-server/how-to-connect-tls-ssl#disable-ssl-enforcement-on-your-flexible-server


![](/img/dbviewer.png)


## Creating Controller and Routes

Next, we are going to generate StudentController, run the below command to create a new controller for PHP CRUD app

```ps
php artisan make:controller StudentController --resource
```

The above command generated a brand new file with this path ```app/Http/Controllers/StudentController.php```. By default, there are seven methods defined in it, which are as follows:

- index() => shows student list
- create() => creates a student using a form
- store() => creates a student in the database
- show() => shows a specific student
- edit() => updates the student data using a form
- update() => updates the student data using a form
- destroy() => removes a particular student

Now, we will start writing the code in ```StudentController.php``` file to initialize the CRUD operations for our PHP app.

```php
<?php

namespace App\Http\Controllers;

use Illuminate\Http\Request;

class StudentController extends Controller
{
    /**
     * Display a listing of the resource.
     *
     * @return \Illuminate\Http\Response
     */
    public function index()
    {
        //
    }

    /**
     * Show the form for creating a new resource.
     *
     * @return \Illuminate\Http\Response
     */
    public function create()
    {
        //
    }

    /**
     * Store a newly created resource in storage.
     *
     * @param  \Illuminate\Http\Request  $request
     * @return \Illuminate\Http\Response
     */
    public function store(Request $request)
    {
        $storeData = $request->validate([
            'name' => 'required|max:255',
            'email' => 'required|max:255',
            'phone' => 'required|numeric',
            'password' => 'required|max:255',
        ]);
        $student = Student::create($storeData);
        return redirect('/students')->with('completed', 'Student has been saved!');
    }

    /**
     * Display the specified resource.
     *
     * @param  int  $id
     * @return \Illuminate\Http\Response
     */
    public function show($id)
    {
        //
    }

    /**
     * Show the form for editing the specified resource.
     *
     * @param  int  $id
     * @return \Illuminate\Http\Response
     */
    public function edit($id)
    {
        $student = Student::findOrFail($id);
        return view('edit', compact('student'));
    }

    /**
     * Update the specified resource in storage.
     *
     * @param  \Illuminate\Http\Request  $request
     * @param  int  $id
     * @return \Illuminate\Http\Response
     */
    public function update(Request $request, $id)
    {
        $updateData = $request->validate([
            'name' => 'required|max:255',
            'email' => 'required|max:255',
            'phone' => 'required|numeric',
            'password' => 'required|max:255',
        ]);
        Student::whereId($id)->update($updateData);
        return redirect('/students')->with('completed', 'Student has been updated');
    }

    /**
     * Remove the specified resource from storage.
     *
     * @param  int  $id
     * @return \Illuminate\Http\Response
     */
    public function destroy($id)
    {
        $student = Student::findOrFail($id);
        $student->delete();
        return redirect('/students')->with('completed', 'Student has been deleted');
    }
}
```

## Configure Routes

Add the following code in ```routes/web.php``` file

```php
<?php

use Illuminate\Support\Facades\Route;
use App\Http\Controllers\StudentController;

/*
|--------------------------------------------------------------------------
| Web Routes
|--------------------------------------------------------------------------
|
| Here is where you can register web routes for your application. These
| routes are loaded by the RouteServiceProvider within a group which
| contains the "web" middleware group. Now create something great!
|
*/

Route::get('/', function () {
    return view('welcome');
});
Route::resource('students', StudentController::class);
```

## Run the following command to create the various routes for our CRUD app.

```ps
php artisan route:list
```

```ps
+--------+-----------+-------------------------+------------------+------------------------------------------------------------+------------------------------------------+
| Domain | Method    | URI                     | Name             | Action                                                     | Middleware                               |
+--------+-----------+-------------------------+------------------+------------------------------------------------------------+------------------------------------------+
|        | GET|HEAD  | /                       |                  | Closure                                                    | web                                      |
|        | GET|HEAD  | api/user                |                  | Closure                                                    | api                                      |
|        |           |                         |                  |                                                            | App\Http\Middleware\Authenticate:sanctum |
|        | GET|HEAD  | sanctum/csrf-cookie     |                  | Laravel\Sanctum\Http\Controllers\CsrfCookieController@show | web                                      |
|        | GET|HEAD  | students                | students.index   | App\Http\Controllers\StudentController@index               | web                                      |
|        | POST      | students                | students.store   | App\Http\Controllers\StudentController@store               | web                                      |
|        | GET|HEAD  | students/create         | students.create  | App\Http\Controllers\StudentController@create              | web                                      |
|        | GET|HEAD  | students/{student}      | students.show    | App\Http\Controllers\StudentController@show                | web                                      |
|        | PUT|PATCH | students/{student}      | students.update  | App\Http\Controllers\StudentController@update              | web                                      |
|        | DELETE    | students/{student}      | students.destroy | App\Http\Controllers\StudentController@destroy             | web                                      |
|        | GET|HEAD  | students/{student}/edit | students.edit    | App\Http\Controllers\StudentController@edit                | web                                      |
+--------+-----------+-------------------------+------------------+------------------------------------------------------------+------------------------------------------+
```

## Create Views in Laravel with Blade Templates

Now, we have to build the views for our student CRUD app with blade files. Go to ```resources/views``` folder and create the following blade templates in our Laravel project.

- layout.blade.php
- index.blade.php
- create.blade.php
- edit.blade.php

### Configure Bootstrap in Laravel
Add the following code in the ```layout.blade```.php template. Here, we defined the main layout for our app along with that we implemented Bootstrap UI framework via Stackpath CDN.

```php
<!DOCTYPE html>
<html>
   <head>
      <meta charset="utf-8">
      <meta name="viewport" content="width=device-width, initial-scale=1">
      <meta http-equiv="X-UA-Compatible" content="ie=edge">
      <title>Laravel 8|7|6 CRUD App Example</title>
      <link rel="stylesheet" href="https://stackpath.bootstrapcdn.com/bootstrap/4.4.1/css/bootstrap.min.css">
   </head>
   <body>
      <div class="container">
         @yield('content')
      </div>
      <script src="https://code.jquery.com/jquery-3.4.1.slim.min.js"></script>
      <script src="https://cdn.jsdelivr.net/npm/popper.js@1.16.0/dist/umd/popper.min.js"></script>
      <script src="https://stackpath.bootstrapcdn.com/bootstrap/4.4.1/js/bootstrap.min.js" type="text/js"></script>
   </body>
</html>
```

In the next step, we will also configure the following views template.

## Create, Store & Validate User Data in MySQL Database
We have to create a form to create, store and validate a user data using Bootstrap Form and Card UI components.

Add the following code in the ```resources/views/create.blade.php``` file.

```php
@extends('layout')
@section('content')
<style>
    .container {
      max-width: 450px;
    }
    .push-top {
      margin-top: 50px;
    }
</style>
<div class="card push-top">
  <div class="card-header">
    Add User
  </div>
  <div class="card-body">
    @if ($errors->any())
      <div class="alert alert-danger">
        <ul>
            @foreach ($errors->all() as $error)
              <li>{{ $error }}</li>
            @endforeach
        </ul>
      </div><br />
    @endif
      <form method="post" action="{{ route('students.store') }}">
          <div class="form-group">
              @csrf
              <label for="name">Name</label>
              <input type="text" class="form-control" name="name"/>
          </div>
          <div class="form-group">
              <label for="email">Email</label>
              <input type="email" class="form-control" name="email"/>
          </div>
          <div class="form-group">
              <label for="phone">Phone</label>
              <input type="tel" class="form-control" name="phone"/>
          </div>
          <div class="form-group">
              <label for="password">Password</label>
              <input type="text" class="form-control" name="password"/>
          </div>
          <button type="submit" class="btn btn-block btn-danger">Create User</button>
      </form>
  </div>
</div>
@endsection
```

To generate a new user, we are using ```create()``` mehtod, which is specified in the ```StudentController.php``` file.

```php
...
/**
     * Show the form for creating a new resource.
     *
     * @return \Illuminate\Http\Response
     */
    public function create()
    {
        return view('create');
    }
    ...
```


and then run 

```ps
php artisan serve
```

go to http://127.0.0.1:8000/students enter value and then submit

![](/img/register-page.png)


Once you click on the submit button, user data is written inside the students table in MySQL db.


![](/img/dbrecord.png)


## Show Users Data

Now, we have to display the users’ data using the Bootstrap table.

The ```index()``` function returns an index view with data retrieved from the mysql database. 

Add the given below code inside the index function.

```php
...
class StudentController extends Controller
{
    /**
     * Display a listing of the resource.
     *
     * @return \Illuminate\Http\Response
     */
    public function index()
    {
        $student = Student::all();
        return view('index', compact('student'));
    }
    ...
```

Add the following code in the ```resources/views/index.blade.php``` file.


```php
@extends('layout')
@section('content')
<style>
  .push-top {
    margin-top: 50px;
  }
</style>
<div class="push-top">
  @if(session()->get('success'))
    <div class="alert alert-success">
      {{ session()->get('success') }}  
    </div><br />
  @endif
  <table class="table">
    <thead>
        <tr class="table-warning">
          <td>ID</td>
          <td>Name</td>
          <td>Email</td>
          <td>Phone</td>
          <td>Password</td>
          <td class="text-center">Action</td>
        </tr>
    </thead>
    <tbody>
        @foreach($student as $students)
        <tr>
            <td>{{$students->id}}</td>
            <td>{{$students->name}}</td>
            <td>{{$students->email}}</td>
            <td>{{$students->phone}}</td>
            <td>{{$students->password}}</td>
            <td class="text-center">
                <a href="{{ route('students.edit', $students->id)}}" class="btn btn-primary btn-sm"">Edit</a>
                <form action="{{ route('students.destroy', $students->id)}}" method="post" style="display: inline-block">
                    @csrf
                    @method('DELETE')
                    <button class="btn btn-danger btn-sm"" type="submit">Delete</button>
                  </form>
            </td>
        </tr>
        @endforeach
    </tbody>
  </table>
<div>
@endsection
```

Check this template on the following URL : http://127.0.0.1:8000/students/

To show the user data in the tabular format, we are going to loop over the students’ array, and don’t forget to add edit and delete button to modify the Laravel app.

![](/img/showallstudent.png)

## Edit and Update Data
To edit and update the user information, we are using the below functions in ```StudentController.php``` file.

```php
...
    public function edit($id)
    {
        $student = Student::findOrFail($id);
        return view('edit', compact('student'));
    }
...
```

To edit and update data in MySQL database we are going to add the following code inside the ```resources/views/edit.blade.php``` file.

```php
@extends('layout')
@section('content')
<style>
    .container {
      max-width: 450px;
    }
    .push-top {
      margin-top: 50px;
    }
</style>
<div class="card push-top">
  <div class="card-header">
    Edit & Update
  </div>
  <div class="card-body">
    @if ($errors->any())
      <div class="alert alert-danger">
        <ul>
            @foreach ($errors->all() as $error)
              <li>{{ $error }}</li>
            @endforeach
        </ul>
      </div><br />
    @endif
      <form method="post" action="{{ route('students.update', $student->id) }}">
          <div class="form-group">
              @csrf
              @method('PATCH')
              <label for="name">Name</label>
              <input type="text" class="form-control" name="name" value="{{ $student->name }}"/>
          </div>
          <div class="form-group">
              <label for="email">Email</label>
              <input type="email" class="form-control" name="email" value="{{ $student->email }}"/>
          </div>
          <div class="form-group">
              <label for="phone">Phone</label>
              <input type="tel" class="form-control" name="phone" value="{{ $student->phone }}"/>
          </div>
          <div class="form-group">
              <label for="password">Password</label>
              <input type="text" class="form-control" name="password" value="{{ $student->password }}"/>
          </div>
          <button type="submit" class="btn btn-block btn-danger">Update User</button>
      </form>
  </div>
</div>
@endsection
```

Start the app in the browser by running the given below command

```ps
php artisan serve
```

## Check out the Laravel App Routes

- Create Student: http://127.0.0.1:8000/students/create
- Students List: http://127.0.0.1:8000/students/

![](/img/Summary.png)
