# App-CVSHARE Backend

![version](https://img.shields.io/badge/version-1.0.0-blue.svg) 
![license](https://img.shields.io/badge/license-MIT-blue.svg)
[![GitHub issues open](https://img.shields.io/github/issues/creativetimofficial/material-dashboard-laravel-livewire.svg)](https://github.com/creativetimofficial/material-dashboard-laravel-livewire/issues?q=is%3Aopen+is%3Aissue) 
[![GitHub issues closed](https://img.shields.io/github/issues-closed-raw/creativetimofficial/material-dashboard-laravel-livewire.svg)](https://github.com/creativetimofficial/material-dashboard-laravel-livewire/issues?q=is%3Aissue+is%3Aclosed)


*Frontend version*: foi usado Material Dashboard v3.0.0.  
 

Speed up your web development with the Bootstrap 5 Admin Dashboard built for Laravel Framework 9.x and up. 

## Table of Contents
* [Prerequisites](#prerequisites)
* [Installation](#installation)
* [Usage](#usage)
* [Versions](#versions)
* [Demo](#demo)
* [Documentation](#documentation)
* [Login](#login)
* [Register](#register)
* [Forgot Password](#forgot-password)
* [Reset Password](#reset-password)
* [User Profile](#user-profile)
* [Dashboard](#dashboard)
* [File Structure](#file-structure)
* [Browser Support](#browser-support)
* [Reporting Issues](#reporting-issues)
* [Licensing](#licensing)
* [Useful Links](#useful-links)
* [Social Media](#social-media)
* [Credits](#credits)

## Prerequisites

If you don't already have an Apache local environment with PHP and MySQL, use one of the following links:

 - Windows: https://updivision.com/blog/post/beginner-s-guide-to-setting-up-your-local-development-environment-on-windows
 - Linux & Mac: https://updivision.com/blog/post/guide-what-is-lamp-and-how-to-install-it-on-ubuntu-and-macos

Also, you will need to install Composer: https://getcomposer.org/doc/00-intro.md  
And Laravel: https://laravel.com/docs/10.x

## Installation
1. Unzip the downloaded archive
2. Copy and paste **material-dashboard-2-free-livewire-master** folder in your **projects** folder. Rename the folder to your project's name
3. In your terminal run `composer install`
4. Copy `.env.example` to `.env` and updated the configurations (mainly the database configuration)
5. In your terminal run `php artisan key:generate`
6. Run `php artisan migrate --seed` to create the database tables and seed the roles and users tables
7. Run `php artisan storage:link` to create the storage symlink (if you are using **Vagrant** with **Homestead** for development, remember to ssh into your virtual machine and run the command from there).


## Usage
Register a user or login with default user **admin@material.com** and password **secret** from your database and start testing (make sure to run the migrations and seeders for these credentials to be available).

Besides the dashboard, the auth pages, the billing and table pages, there is also has an edit profile page. All the necessary files are installed out of the box and all the needed routes are added to `routes/web.php`. Keep in mind that all of the features can be viewed once you login using the credentials provided or by registering your own user. 


## Demo
| Register | Login | Dashboard |
| --- | --- | ---  |
| [<img src="https://github.com/AlexVlad95/material-2-free-livewire/blob/master/screens/register.png" width="322" />](https://material-dashboard-laravel-livewire.creative-tim.com/sign-up) | [<img src="https://github.com/AlexVlad95/material-2-free-livewire/blob/master/screens/login.png" width="322" />](https://material-dashboard-laravel-livewire.creative-tim.com/sign-in)  | [<img src="https://github.com/AlexVlad95/material-2-free-livewire/blob/master/screens/dashboard.PNG" width="322" />](https://material-dashboard-laravel-livewire.creative-tim.com/dashboard)

| Forgot Password Page | Reset Password Page | Profile Page  |
| --- | --- | ---  |
| [<img src="https://github.com/AlexVlad95/material-2-free-livewire/blob/master/screens/forgot-password.png" width="322" />](https://material-dashboard-laravel-livewire.creative-tim.com/forgot-password)  | [<img src="https://github.com/AlexVlad95/material-2-free-livewire/blob/master/screens/reset-password.png" width="322" />](https://material-dashboard-laravel-livewire.creative-tim.com/sign-in) | [<img src="https://github.com/AlexVlad95/material-2-free-livewire/blob/master/screens/profile.png" width="322" />](https://material-dashboard-laravel-livewire.creative-tim.com/user-profile)
[View More](https://material-dashboard-laravel-livewire.creative-tim.com/dashboard)

## Documentation
The documentation for the Material Dashboard Laravel is hosted at our [website](https://material-dashboard-laravel-livewire.creative-tim.com/documentation/getting-started/installation.html).

### Login
If you are not logged in you can only access this page or the Sign Up page. The default url takes you to the login page where you use the default credentials **admin@material.com** with the password **secret**. Logging in is possible only with already existing credentials. For this to work you should have run the migrations. 

The `App/Http/Livewire/Auth/Login.php` handles the logging in of an existing user.

```
       public function store()
                {
                    $attributes = $this->validate();
            
                    if (! auth()->attempt($attributes)) {
                        throw ValidationException::withMessages([
                            'email' => 'Your provided credentials could not be verified.'
                        ]);
                    }
            
                    session()->regenerate();
            
                    return redirect('/dashboard');
            
                }
```

### Register
You can register as a user by filling in the name, email and password for your account. You can do this by accessing the sign up page from the "**Sign Up**" button in the top navbar or by clicking the "**Sign Up**" button from the bottom of the log in form.. Another simple way is adding **/sign-up** in the url.

The `App/Http/Livewire/Auth/Register.php` handles the registration of a new user.

```
   public function store(){

                  $attributes = $this->validate();
          
                  $user = User::create($attributes);
          
                  auth()->login($user);
                  
                  return redirect('/dashboard');
              } 
```

### Forgot Password
If a user forgets the account's password it is possible to reset the password. For this the user should click on the "**here**" under the login form.

The `App/Http/Livewire/Auth/ForgotPassword.php` takes care of sending an email to the user where he can reset the password afterwards.

```
       public function show(){
          
                  $this->validate();
          
                  $user = User::where('email', $this->email)->first();
          
                      if($user){
          
                          $this->notify(new ResetPassword($user->id));
          
                          return back()->with('status', "Email sent.");
          
                      } else {
                          return back()->with('email', "Invalid email");
                      }
              }
```

### Reset Password
The user who forgot the password gets an email on the account's email address. The user can access the reset password page by clicking the button found in the email. The link for resetting the password is available for 12 hours. The user must add the email, the password and confirm the password for his password to be updated.

The `App/Http/Livewire/Auth/ResetPassword.php` helps the user reset the password.

```
     public function update(){
        
                $this->validate(); 
                  
                $existingUser = User::where('email', $this->email)->first();
        
                if($existingUser && $existingUser->id == $this->urlID) { 
                    $existingUser->update([
                        'password' => $this->password
                    ]);
                    redirect('sign-in')->with('status', 'Password changed.');
                } else {
                    return back()->with('email', "Invalid email");
                }
            
            }
```

### User Profile
The profile can be accessed by a logged in user by clicking "**User Profile**" from the sidebar or adding **/user-profile** in the url. The user can add information like phone number, location, description or change the name and email.

The `App/Http/Livewire/ExampleLaravel/UserProfile.php` handles the user's profile information.

```
    public function update()
                {
                    $this->validate();
                    $this->user->save();
                    return back()->withStatus('Profile successfully updated.');
                
                 }
}
```

### Dashboard
You can access the dashboard either by using the "**Dashboard**" link in the left sidebar or by adding **/dashboard** in the url after logging in. 

## File Structure
```
+---app
|   +---Console
|   |       Kernel.php
|   +---Exceptions
|   |       Handler.php
|   +---Http
|   |   +---Controllers
|   |   |       Controller.php
|   |   |       
|   |   +---Middleware
|   |   |       Authenticate.php
|   |   |       EncryptCookies.php
|   |   |       PreventRequestsDuringMaintenance.php
|   |   |       RedirectIfAuthenticated.php
|   |   |       TrimStrings.php
|   |   |       TrustHosts.php
|   |   |       TrustProxies.php
|   |   |       VerifyCsrfToken.php
|   |   |
|   |   +---Livewire
|   |   |   | 
|   |   |   +---Auth
|   |   |   |     ForgotPassword.php
|   |   |   |     Login.php
|   |   |   |     Logout.php
|   |   |   |     Register.php
|   |   |   |     ResetPassword.php
|   |   |   | 
|   |   |   +---ExampleLaravel 
|   |   |   |     UserManagement.php
|   |   |   |     UserProfile.php
|   |   |   |
|   |   |   |   Billing.php
|   |   |   |   Dashboard.php
|   |   |   |   Notifications.php
|   |   |   |   Profile.php
|   |   |   |   RTL.php
|   |   |   |   StaticSignIn.php
|   |   |   |   StaticSignUp.php
|   |   |   |   Tables.php
|   |   |   \   VirtualReality.php
|   |   |   
|   |    \---Kernel.php   
|   |   
|   +---Models
|   |        User.php
|   |
|   +---Notifications
|   |        ResetPassword.php
|   |     
|   |---Proviers
|   |      AppServiceProvider.php
|   |      AuthServiceProvider.php
|   |      BroadcastServiceProvider.php
|   |      EventServiceProvider.php
|   |      RouteServiceProvider.php
|   | 
|   \---View
|          App.php
|          Base.php
|   
....

```

## Browser Support
At present, we officially aim to support the last two versions of the following browsers:

<img src="https://s3.amazonaws.com/creativetim_bucket/github/browser/chrome.png" width="64" height="64"> <img src="https://s3.amazonaws.com/creativetim_bucket/github/browser/firefox.png" width="64" height="64"> <img src="https://s3.amazonaws.com/creativetim_bucket/github/browser/edge.png" width="64" height="64"> <img src="https://s3.amazonaws.com/creativetim_bucket/github/browser/safari.png" width="64" height="64"> <img src="https://s3.amazonaws.com/creativetim_bucket/github/browser/opera.png" width="64" height="64">

## Credits

- [Creative Tim](https://creative-tim.com/?ref=md2ll-readme)
- [UPDIVISION](https://updivision.com)
