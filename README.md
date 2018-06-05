[![StyleCI](https://gitlab.styleci.io/repos/6754365/shield?branch=master)](https://gitlab.styleci.io/repos/6754365)

[![SensioLabsInsight](https://insight.sensiolabs.com/projects/82e35aa0-2a67-42dd-9aae-4eeb0ec84fdb/big.png)](https://insight.sensiolabs.com/projects/82e35aa0-2a67-42dd-9aae-4eeb0ec84fdb)


[![Latest Version on Packagist](https://img.shields.io/packagist/v/secmohammed/artify.svg?style=flat-square)](https://packagist.org/packages/secmohammed/artify)

[![Total Downloads](https://img.shields.io/packagist/dt/secmohammed/artify.svg?style=flat-square)](https://packagist.org/packages/secmohammed/artify)

<p align="center"> Made with ❤️ by  SecTheater Foundation</p>

> Artify CheatSheet will be availble soon 


<hr>

<p>Laravel is a great framework and it provides us to create our custom artisan commands, so why not to have a couple of commands that makes your life easier while develping your application. </p>
<b>If you have any inspiration about a new command , or you do something routinely and you want a command for that , Please do not hesitate at all.</b>
<p>Artify cares about the commands that you should have within your application, for now, it ships with commands that will help you to create files that laravel don't supply a command for them till now such as Repositories,Observers,Responsable Interface,facades and so on..</p>

<h1>If you have any inspiration to create a new command, or something you routinely do , you can tell us about that, and we will make it happen</h1>
## Installation Steps

### 1. Require the Package

After creating your new Laravel application you can include the Jarvis package with the following command: 

```bash
composer require secmohammed/artify:dev-master
```
If your laravel version is 5.5+, you don't need to register ArtifyServiceProvider as It"s automatically discovered, unless that you got to register it manually in your config/app.php

```
"providers" => [
   // other providers
   Artify\Artify\ArtifyServiceProvider::class
]
```
### 2. Add the DB Credentials & APP_URL

Next make sure to create a new database and add your database credentials to your .env file:

```
DB_HOST=localhost
DB_DATABASE=homestead
DB_USERNAME=homestead
DB_PASSWORD=secret
```

You will also want to update your website URL inside of the `APP_URL` variable inside the .env file:

```
APP_URL=http://localhost:8000
```

### 3. Getting Your Environment Ready.

#### Just Run The following command.


```bash
	php artisan artify:install -c 20
```

##### Command Interpretation

1- It publishes the roles migrations,its seed, and the config file that artify will use.
<p>
2- "-c" stands for the caching whether to enable or not, 20 stands for the number of minutes that's going to be cached.

</p>
<p>If you don't want to enable the cache just remove the -c and the number to become like that</p>


```

php artisan artify:install

```


3- It asks about the Role model, and the permissions column which will artify handle during the generating files related to roles.


> Artify Provides you the ability to set your custom namespace and your custom user model within config/artify.php


![Installation Preview](http://sectheater.org/assets/images/doc/artify.png)

Now everything is set, Let"s dive into the Commands.
### 4. Commands that artify covers.
<hr>

#### 4.1 Create an Observer
Observer is typically observing for a particular eloquent event. On happening this event, Observer is fired, You can read about it at the documentation.


```bash
  // ? means optional.
 // php artisan artify:observer <name of Observer> <model attached?> <methods to ignore while creating observer?>
 // Example 1.0
 php artisan artify:observer Post --class="Post" created updated
```
The upon command generates an observer file within your App/Observers with name of PostObserver
The methods there now have Post Model as parameter passed,
The ignored methods while creating the observer ( which won't be within this file) are created and updated.
<hr>


#### 4.2 Create A Responsable Interface 


Responsable Interface is a Laravel 5.5 feature that you should use to make your controller slim, you can see a tutorial about it.
https://www.youtube.com/watch?v=yKNK6MZdSrY

```bash
	php artisan artify:response <name>
	// Example , please try to follow the convention of naming.. <model><method>Response
    php artisan artify:response PostShowResponse
```
The command upon is going to create you a new file with the name you attached under App/Responses
<hr>


#### 4.3 Synchronozing Your Roles table with Policy & Gates


<b>This command is in charge of converting the available roles within your database into policy and gates</b>
<p>It"s preferable to follow the convention of naming the roles as mentioned in the Roles seeder</p>
<p>Artify also supplies you with the migration for roles, if you are going to change the permissions column, don't forget to update that within the config file</p>
<p>Permissions column should be an array, if they are jsonb in your database, use the accessors, or observers, or use casts property to convert permissions to an array</p>


```bash

// Role Model

protected $casts = ["permissions" => "array"];

```


<p>By Default this command requires the hasRole method ( it should return boolean ) , you can create your custom one within user model, or just import the Roles trait</p>


```
 use Artify\\Artify\\Traits\\Roles\\Roles;

 Class User Extends Model {
   use Roles;    
 }
```


This method is required to check whether the user has a specific role to access somewhere or not, It"s used within the gates.
Feel free to change the gates"s logic to suit your needs.

```
  php artisan artify:register-authorization
```

By Firing this command, you can use the can middleware anywhere within your route file
```
  Route::get("/post/create","PostController@create")->middleware("can:create-post");
```
also you can access the method can anywhere in your project.
```bash
 if($user->can("create-post"))
 // do something
```
<hr>


#### 4.4 Assign User To A Specific Rank.


This command is just there to save you a couple of minutes whenever you assign a user manually to a specific role through database.
```bash
 php artisan artify:assign <username> <slug>
 //  Example : 
 php artisan artify:assign Alex Admin
```
<hr>


#### 4.5 Create A Repository


Repository patten is absolutely a powerful pattern that you should use in order to separate logic from your database layer ( model ).

```bash
 php artisan artify:repository <name of repo> (-m?) (-f?)
 // -m stands for the model associated with the repository.
 // -f create a facade for this repository, so you can call it immediately , i.e you can just do that anywhere "\MyRepository::method();"
```
<hr>


#### 4.6 Generate a facade.


```bash

php artisan artify:facade <name>

```

<hr>


#### 4.7 Generate CRUD


Well, This artifier is really a beast, it could save you up to 20 minutes or something , It helps you to generate the following 
 - Model
 - Resource Controller with the common logic you mostly do  within your methods.
 - Request Form files for your store and update methods.
 - Resource Route within your web file
 - If Repository Option is enabled, It will create you the repository associating with the model.
 

```bash
 php artisan artify:crud <model-name> (-r?)
 // -r is optional , it will generate you the repository.
```
<hr>


### 5.0 Roles Trait


This trait is there to handle the authorization process for artify commands , but feel free to use it for your own purposes.
It contains of a few methods that"s really helpful for you to handle authorization anywhere within your application.
Let"s dive into the Methods there.
> The Trait supports checking roles within your roles table and the users table ( secondary permissions to be assigned for users )  

#### Authorization Methods. 

##### 1.0 hasRole Method
  <p>This method accepts only one argument and it should be string, the string should match a role name within your permissions column either on users table or roles table.</p>
  
  ```bash
  
  $user = \App\User::first();
  
  dd($user->hasRole("create-post")); // if the current logged in user has the ability to "create-post", it will return true
  
  ```
  
  
  The method is going to search for the permission associated with the user either on roles table or users table.
##### 2.0 hasAnyRole Method
  <p>I think you"ve guessed what"s going on here, It searches for all of the roles you pass and returns true on the first role that a user may have</p>
  
  ```bash
    dd($user->hasAnyRole(["create-post","foo-bar","approve-post"])); // returns true/false;
  ```
##### 3.0 hasAllRole Method
 <p>This method accepts one argument, it should be an array containing of the permissions you are checking for. </p>
 This method checks on all the roles passed, whether the user has them all or not.
 
 ```bash 
   dd($user->hasAllRole(["foo-bar","upgrade-user"])); // returns true/false;
 ```
 
##### 3.0 inRole Method
 <p>This method checks if the user"s rank is typically equal to the passed argument. </p>
 - Slug that represents your permissions in role table.
 - returns boolean 

  
```bash
 dd($user->inRole("admin"); // This slug exists within the seeder
```

<hr>

#### Permissions Handling Methods.
<b>The permissions are assigned to the current user you are working on only. i.e you can retrieve the user by finding him/her or get the authenticated user then handle his/her permisisons.</b>
##### 1.0 Add Permission
   
   <p>This method accepts two arguments which  are the permissions and boolean value.</p>
   - Permissions can be either string or array.
   - boolean value represents the ability of the user to do mentioned permission.
   - returns boolean.


  ```bash
     $user->addPermission("create-custom-post"); // second parameter is set to true by default, so the added permission is available for this user.
     $user->addPermission("create-something",false) // the user isn\'t allowed to "create-something" now.
     $user->addPermission(["create-something" => true , "can-push-code" => false]) 
     /* you don\'t need the second parameter now as the key and value of this array is going to be in charge of handling the permissions. */
     
  ```
  
  <hr>

##### 2.0 Update Permission
 
<p> This method accepts three arguments</p>
 - Permission , should be a string.
 - boolean value represents the ability of the user to do mentioned permission.
 - boolean value , represents creating the permission if it doesn't exist.
 - returns boolean.


 ```bash
  $user->updatePermission("create-post"); // this will update the permission to set it to true.
  $user->updatePermission("create-post",false); // this will update the permission to set it to false
  $user->updatePermission("foo-bar",false,true); // this will create the permission if it doesn\'t exist and set it to false.
 ```
 
 <hr>

##### 3.0 Remove Permission
 <p>This method accepts one argument only representing the permission</p>
 - n of permissions can be passed a separate parameter.
 - returns boolean


 <p>If the permission isn't set, an exception is thrown.</p>

 ```bash
   $user->removePermission("create-post"); // returns boolean
   $user->removePermission("create-post","delete-post","whatever-role",...); // returns boolean
 ```
