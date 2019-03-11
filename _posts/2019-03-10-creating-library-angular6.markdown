---
layout: post
title:  "Creating and sharing a library in Angular 6"
categories: Angular6
image:  /preview.jpg
---

Angular 6 brought a bunch of new features with it's release and one of those features is the ability to create and consume custom libraries.
These libraries can constitute a whole range of functionalities ranging from shareable components to shareable services.

I am going to showcase a step by step guide which would enable you to start writing a simple library(which you can later build upon as per your requirement) and way to consume this library into applications(I will explain about applications as we proceed).

So let's write some code!

<h2><strong>Getting started</strong></h2>
Before starting with the creation of library and applications, make sure you have Angular 6+ installed.

One of the other new features of Angular 6 is the ability to create multiple applications in a workspace or a single source structure.

This type of structure is particularly useful when we intend to create a dashboard/portal type of project having multiple pages which do not rely on each other but can have some common features. These common features can be built into a library and shared among these pages.
For this simple walkthrough, we'll create an Employee dashboard having the following projects:

1. Timesheet application
2. Worksheet application
3. Shared library

Open any preferred command line(I am using Visual Studio Code terminal), browse to the directory in which you want to create the Employee Portal projects and type:

{% highlight ruby %}
ng new employee-portal
{% endhighlight %}

We will see something like below when we hit enter:

<img src="/images/post1-creating-library-angular/screenshot-1.png">

Post success of the above command, a new angular project gets created which serves as the container for the applications we are going to create next. The project will have the following file structure:

<img src="/images/post1-creating-library-angular/screenshot-2.png">

We would not be needing the highlighted "src" folder and can remove this later.


<h2><strong>Create Workitems project</strong></h2>

To create our first project, we type the below command on the terminal. Here "application" schematic specifies that we are going to create an application followed by the application name.

{% highlight ruby %}
ng generate application workitems
{% endhighlight %}

<img src="/images/post1-creating-library-angular/screenshot-3.png">

Post success of the above command, Angular CLI creates a folder named "projects". Our "workitems" folder gets created inside this "projects" folder and has a "src" folder which will contain all the code for "workitems".

<img src="/images/post1-creating-library-angular/screenshot-4.png">

<h2><strong>Create Timesheet project</strong></h2>

Similar to how we created "workitems", follow below command to create "timesheet" project.

{% highlight ruby %}
ng generate application timesheet
{% endhighlight %}

<img src="/images/post1-creating-library-angular/screenshot-5.png">

"timesheet" gets created adjacent to "workitems" project.

<img src="/images/post1-creating-library-angular/screenshot-6.png">

If at this point we open the "angular.json" file at the root of employee-portal, we will see that a section gets created for our newly created projects/applications.

<img src="/images/post1-creating-library-angular/screenshot-7.png">

Each project has quite a bit of configurations. Please note that each project has a separate "outputPath", so when these projects are built, each would have a separate output folder having a separate "index.html".

<img src="/images/post1-creating-library-angular/screenshot-8.png">

<h2><strong>Running Applications</strong></h2>

To run our "timesheet" application we will use our good old "ng serve" along with the project name that we want to run.

{% highlight ruby %}
ng serve timesheet
{% endhighlight %}

<img src="/images/post1-creating-library-angular/screenshot-9.png">

Post successful build, open any preferred browser on http://localhost:4200/.
We will see a simple page displaying the project heading.

<img src="/images/post1-creating-library-angular/screenshot-10.png" class="fit image">

To see how workitems look, run below command. I have not used a separate port for "workitems". But you can go ahead and use a different port in order to see the two applications in the browser.

{% highlight ruby %}
ng serve workitems
{% endhighlight %}

To use a particular port we can run:

{% highlight ruby %}
ng serve workitems — port 4202
{% endhighlight %}

<img src="/images/post1-creating-library-angular/screenshot-11.png">

<img src="/images/post1-creating-library-angular/screenshot-15.png" class="fit image">

<h2><strong>Create Shareable Library</strong></h2>

To create a library we use the schematic "library". Here "g" is the short form for "generate" and "-p emp" tells the Angular CLI to use "emp" as the selector prefix.

{% highlight ruby %}
ng g library emp-shared -p emp
{% endhighlight %}

Post the success of above command, a folder for emp-shared gets created in the "projects" folder.

<img src="/images/post1-creating-library-angular/screenshot-12.png">

The CLI has added a "paths" section to tsconfig.json in the root of employee-portal.

<img src="/images/post1-creating-library-angular/screenshot-13.png">

This change to tsconfig.json will allow our other projects to access our library at development time.
A section for "emp-shared" project gets added to "angular.json" similar to "workitems" and "timesheet".
Notice the "projectType" is "library" for our library project.

<img src="/images/post1-creating-library-angular/screenshot-14.png">

<h2><strong>Create Component and Service in the Library</strong></h2>

We will create a component and a service in the library. The service will be responsible for getting the details of the currently logged-in user in the portal. The component will display this information on a page with the help of html selector. The selector can then be consumed by "workitems" and "timesheet" projects as this information is something which is common for both the applications.

In order to create a component inside the library, either we have to change the directory from the root to "emp-shared" or we can use the below command by specifying the project name while running the command in the root folder.

{% highlight ruby %}
ng generate component currentUser --project=emp-shared
{% endhighlight %}

<img src="/images/post1-creating-library-angular/screenshot-16.png">

The CLI will create the component inside "emp-shared" as shown below:

<img src="/images/post1-creating-library-angular/screenshot-17.png">

The "emp-shared.module.ts" looks like below at the moment:

{% highlight ts %}
import { NgModule } from '@angular/core';
import { EmpSharedComponent } from './emp-shared.component';
import { CurrentUserComponent } from './current-user/current-user.component';

@NgModule({
  declarations: [EmpSharedComponent, CurrentUserComponent],
  imports: [
  ],
  exports: [EmpSharedComponent]
})
export class EmpSharedModule { }
{% endhighlight %}

To create a service in the library we use below command by specifying "service" schematic.

{% highlight ruby %}
ng g service userDetails --project=emp-shared
{% endhighlight %}

I have used hard coded values in the service's "getLoggedInUser" method, but you can call an API to get the details accordingly.

{% highlight ts %}
import { Injectable } from '@angular/core';
import { User } from './User.js';

@Injectable({
  providedIn: 'root'
})
export class UserDetailsService {

  constructor() { }

  getLoggedInUser(): User{
    let user = new User();
    user.name = "Neha Upadhyay";
    user.title = "Developer";
    user.department = "Software";

    return user;
  }
}
{% endhighlight %}

In "current-user" component, we call the service's "getLoggedInUser" method and assign the returned values to the class variables. We then call this method in ngOnInit(). This will ensure then whenever we use this component's selector in any other application we will automatically trigger this method and the values will be available to us.

Note the selector for the component is "emp-current-user". While creating the library we used "emp" as the prefix for selectors.

{% highlight ts %}
import { Component, OnInit } from '@angular/core';
import { UserDetailsService } from '../user-details.service';

@Component({
  selector: 'emp-current-user',
  templateUrl: './current-user.component.html',
  styleUrls: ['./current-user.component.css']
})
export class CurrentUserComponent implements OnInit {

  name: string;
  title: string;
  department: string;
  time: Date;
  constructor(private userService: UserDetailsService) { }

  ngOnInit() {
    this.getLoggedInUser();
  }

  getLoggedInUser() {
    let data = this.userService.getLoggedInUser();
    this.name = data.name;
    this.title = data.title;
    this.department = data.department;
    this.time = new Date();
  }
}
{% endhighlight %}

We then bind the class variables in the html.

{% highlight html %}
<p>
  The current logged in user is <strong>{{"{{name"}}}}</strong>.<br />
  This user is a <strong>{{"{{title"}}}}</strong> and belongs to <strong>{{"{{department"}}}}</strong> department.<br />
  Logged in time: <strong>{{"{{time"}}}}</strong>
</p>
{% endhighlight %}

We need to export "CurrentUserComponent" in "EmpSharedModule".

{% highlight ts %}
import { NgModule } from '@angular/core';
import { EmpSharedComponent } from './emp-shared.component';
import { CurrentUserComponent } from './current-user/current-user.component';

@NgModule({
  declarations: [EmpSharedComponent, CurrentUserComponent],
  imports: [
  ],
  providers: [],
  exports: [EmpSharedComponent, CurrentUserComponent]
})
export class EmpSharedModule { }
{% endhighlight %}

In the public_api.ts we need to export all the components that we want to consume in other applications.
For this add below line of code in the file.

{% highlight ts %}
export * from './lib/current-user/current-user.component';
{% endhighlight %}

<h2><strong>Building the library</strong></h2>

To build the library, browse to the root of the angular project which is "employee-portal" in our case, if you are not already there.

Run below command.

{% highlight ruby %}
ng build emp-shared
{% endhighlight %}

A folder for "emp-shared" gets created in the "dist" folder.

<img src="/images/post1-creating-library-angular/screenshot-18.png">

<h2><strong>Installing the library</strong></h2>

To make the library available for other applications, we need to first install it using the below command.

{% highlight ruby %}
npm install dist/emp-shared
{% endhighlight %}

This adds an entry for the library package in "package.json" file, and adds a link from the node-modules directory to the dist/emp-shared directory.

<img src="/images/post1-creating-library-angular/screenshot-19.png">

NOTE : Don't forget to rebuild the library after every update in order to make the changes reflect in the consumer projects. We don't need to do npm install again.

<h2><strong>Consuming the library</strong></h2>

Our next step is to use the current-user component from the library in our "workitems" and "timesheet" applications.
For this, in app.module.ts add below line.

{% highlight ts %}
import { EmpSharedModule } from 'emp-shared';
{% endhighlight %}

and in the "imports" section add EmpSharedModule

{% highlight ts %}
imports: [
    BrowserModule,
    EmpSharedModule
  ]
{% endhighlight %}

In app.component.html for both the applications add the below selector.

{% highlight html %}
<emp-current-user></emp-current-user>
{% endhighlight %}

Now we are ready to see the data returned from our library to our consumer applications.

{% highlight ruby %}
ng serve timesheet
{% endhighlight %}

<img src="/images/post1-creating-library-angular/screenshot-20.png" class="fit image">

{% highlight ruby %}
ng serve workitems
{% endhighlight %}

<img src="/images/post1-creating-library-angular/screenshot-21.png" class="fit image">

<h2><strong>Summary</strong></h2>

With this we have completed the development of a basic shareable library in Angular 6. The components and applications created are not useful functionally but hopefully I am able to explain the technique to create multiple applications in a single code structure and a shareable library for the consumer applications.

<h2><strong>Final Notes</strong></h2>

There are some important points I would like to highlight which are likely to be missed by us.

1. Rebuild the library after every change.
2. Add the exports in public_api.ts file for all the features we want to export. This can include components, services, model classes, directives etc.
3. If you download the code from github, you cannot directly run npm install as the package.json contains the line to install "emp-shared" from dist folder but that won't be available post the download. To resolve this, follow below mentioned steps in the order they are specified.
  - Remove reference of "emp-shared" from package.json.
  - Delete package-lock.json file.
  - Run npm install
  - Run ng build emp-shared
  - Run npm install dist/shared


<b>Finally</b>, I would appreciate your comments, feedback and suggestions on the post.
