# Code and Theme Packages for local and remote access - Tutorial

## Table of Contents

	-	Pre-Requisites
	-	About this Demo
	-	Introduction
	-	Overview
	-	The Demo
	-	Part 1 - The initial Workspace with the code and theme packages
	-	Part 2 - Setting up the theme package and code package to be used for "local" repo
	-	Part 3 - Setting Up the packages to be distributed via "remote" and use these packages remotely
	-	References

## Pre-Requisites

You will need the following installed and available:

	-	Sencha Command version 6.2.x
	-	ExtJS version 6.2.x
	-	A Text Editor
	-	Cmd Window(s)
	-	Administrative privilieges to your system is desirable
	-	Web Server space to load the packages for remote access
	-	An FTP client may be required to upload your packages to a remote server


## About this Demo

This was designed using 2 PCs

	-	A Windows 7 Professional version to develop most of the demos
	-	A Windows 10 Professional version to use the remote package

This demo creates a theme package and a code package for use with the `classic` mode of `ExtJS version 6.2.x`.
The version of Windows aren't important, you can also easily run this demo for Mac and Linux, for the most part, typically, it's all about running command either as an admin more than likely using 'sudo'.


## Introduction

Packages are the way to distribute code and styling to other Sencha apps.

These are created and managed using Sencha Command.

It is important to understand that for package management to function correctly, Sencha Command must be able to read and write to your local file system.

For this demo, it is important to ensure that Sencha Cmd has full disk permissions within the directory it is located in and any of its subdirectories.

This demo was created on a Windows 7 machine where Sencha is located at:  `c:\Users\<Machine User>\bin\Sencha`


## Overview

This demo deals with the main logistics revolving around packages:

	-	Package types
	-	How Sencha Command looks for packages when building an app


### Package types

We are dealing with 2 types of packages in this document.

	-	theme package
	-	code package

Note: there will be more links and references about Sencha Cmd and Packages where you can see all the different types of packages available, but for the most part, theme and code are the most common ones.


#### Theme Package

This type of package allows the distribution of styles and their resources (images, etc.)

#### Code Package

This type of package allows the distribution of code snippets and classes along with any other resources (js files, etc.)

### How Sencha Command looks for packages when building an app

Code and style reuse is obviously a good thing, leveraging existing work as reusable components allows to promote consistency.

Packages are required or set as theme in your `app.json` file.

`Sencha Cmd` looks for these packages by reference in well defined order.

The following is how Sencha resolves the package dependency:

	1) First, the workspace the app is part of "(packages\local\<required package>"; then
	2) the local repo located at "bin\Sencha\Cmd\repo\pkgs"; and then
	3) look at a set of remote repositories defined to see if it has the package; and last;
	4) Download the package from the remote repository and add it to the local repository

To test if you have any remote/local repos, you can open a command prompt window and test it using the following command:
`sencha repository list`

Note: You always have at least 1 remote repo with Sencha, the `sencha` repo: `sencha - http://cdn.sencha.com/cmd/packages`


## The Demo (step-by-step)

The Demo is broken down into 3 parts and uses 2 PCs

	-	"PC 1": The PC where most of the coding and implemention is done
	-	"PC 2": The PC which will be used to test remote access to the packages

Note: ideally, 2 PCs is the best way to ensure that 'remote' access is fully tested.  But you can always use tools like VirtualBox to open other OS sessions which would be just as valid, truly, at this point, the bottom line is that because of how Sencha resolves dependency, a single PC which is used to create and manage the repo/packages will never load from remote.


### Part 1 - The initial Workspace with the code and theme packages

	1) Create a Workspace
	2) Create a theme package
	3) Configure your package.json file for your theme package
	4) Add some SASS to style "Ext.Component" and "Ext.panel.Panel" components
	5) Clean and build your theme package
	6) Create a code package
	7) Configure your "package.json" file for your code package
	8) Add a custom Panel class
	9) Clean and build your code package
	10) Create a sample app and use both theme and code package

The end result will be a simple app which will use within its Workspace a code package and a theme package.

### Part 2 - Setting up the theme package and code package to be used for "local" repo

	1) Initialize a repo
	2) Verify the reference in "sencha.cfg"
	3) Add each package to your local repo
	4) Verify your packages are added to the local repo
	5) Verify that sencha can report a list of packages

### Part 3 - Setting Up the packages to be distributed via "remote" and use these packages remotely

	1) Upload the files to the web server.
	2) Set up a PC for remote access to our Sencha repo


## Part 1 - The initial Workspace with the code and theme packages

Note: Until noted otherwise, you will use `PC 1`.

	1) Create a Workspace

Type: `sencha -sdk <path to extjs> generate workspace PackageProviderDemo`

This will create a directory and workspace and copy the extjs framework in an ext folder inside the workspace directory

	2) Create a theme package

Note: everything we do is case sensitive, it is recommended you keep your package name in lowercase.

Inside the workspace directory, create a theme package as follows:

Type: `C:\PackageProviderDemo>sencha generate package -type theme packagetheme`

This will create a package named `packagetheme` inside your Workspace under the `packages\local\` folder

	3) Configure your "package.json" file for your theme package

The following configs are the most important ones to pay attention too, ensure for this demo you have the following values set:

	

	"name": "packagetheme",

	"namespace": "Ext",
	
	"framework": "ext",
	
	"type": "theme",
	
	"extend": "ext-theme-neptune",
	
	"toolkit": "classic",
	
	"creator": "demo",
	
	"version": "1.0.0",
    

"name":			this should be identical to your folder name for your package

"namespace":	you are styling Ext components, so, it should make the root namespace of Ext which is 'Ext'

"framework":	should be a valid framework as defined in your Workspace.json file, typically 'ext'

"type": 		"theme" goes without saying

"extend":		this should be a valid theme as you have either created yourself (yes, you can extend from a custom one, or a valid theme which is part of the framework you have targetted.  For example, look under your PackageProviderDemo\ext\classic\ and you will see many theme packaged available.

"toolkit":		in our case it is "classic", but it could be "modern" if you were building HTML5 based apps

"creator":		in future steps, we will be using this "creator" config as a means of identifying a repo, so, by setting this config to "demo", you are telling Sencha Cmd that it will be part of the "demo" repo.

"version":		you can actually build different versions of your theme and deploy them in various ways, this stays as is, and it is beyond this tutorial's purpose, but there are links at the end of this document where you can read more about this feature.


	4) Add some SASS to style "Ext.Component" and "Ext.panel.Panel" components

In your `packagetheme` folder under `packages\local` you will find many folders.

For now, look for a `sass\var\` folder

We will add a file `Component.scss` and a folder `panel`, followed by another file inside of the panel folder called `Panel.scss`.

Inside the `Component.scss` file, we will add a SASS variable which will influence all `Ext.Component` instances and those which inherit from it.

Type the following and save the file:  `$base-color: #006600 !default;`

Inside the `panel\Panel.scss` file, we will add 2 SASS variables which will influence all `Ext.Panel` instances and those which inherit from it.

Type the following and save the file:

`$panel-body-color: #FFFFFF !default;

$panel-body-background-color: #000066 !default;`


	5) Clean and build your theme package

At this point, all your content is set, you need only to clean your package and build it.

At your package's directory `C:\>PackageProviderDemo\packages\local\packagetheme>`
Type: `sencha ant clean`
then type:
Type: `sencha package build`


	6) Create a code package

Note: everything we do is case sensitive, it is recommended you keep your package name in lowercase.

Inside the workspace directory, create a code package as follows:

Type: `C:\PackageProviderDemo>sencha generate package -type code packagecode`

This will create a package named `packagetheme` inside your Workspace under the `packages\local\` folder

	7) Configure your "package.json" file for your code package

The following configs are the most important ones to pay attention too, ensure for this demo you have the following values set:

   
	
	"name": "packagecode",
	
	"namespace": "PackageCode",
	
	type": "code",
	
	"creator": "demo",
	
	"theme": "ext-theme-neptune",
	
	"framework": "ext",
	
	"toolkit": "classic",
	
	"version": "1.0.0",



"name":			this should be identical to your folder name for your package

"namespace":	This will be the name of the root of your classes for all code you share (components, etc.)  It should be CamelCase to follow the Sencha naming convention, so, since our demo uses "packagecode" as a name, we should call our namespace "PackageCode".

"type": 		"code" goes without saying

"creator":		in future steps, we will be using this "creator" config as a means of identifying a repo, so, by setting this config to "demo", you are telling Sencha Cmd that it will be part of the "demo" repo.

"theme":		put a valid theme, but know that when you require this package for an app, the app's theme will override this theme.

"framework":	should be a valid framework as defined in your Workspace.json file, typically 'ext'

"toolkit":		in our case it is "classic", but it could be "modern" if you were building HTML5 based apps

"version":		you can actually build different versions of your code package and deploy them in various ways, this stays as is, and it is beyond this tutorial's purpose, but there are links at the end of this document where you can read more about this feature.



	8) Add a custom Panel class

In your `packagecode` folder under `packages\local` you will find many folders.

For now, look for a `src\` folder

We will add a folder `panel`, followed by a file inside of the panel folder called `Panel.js`.

Inside the `Panel.js` file we will add the following content:



	Ext.define('PackageCode.panel.Panel', {

	  extend: 'Ext.panel.Panel',
	  
	  xtype: 'packagecode-panel',
	  
	  title: "My Default Panel"
	  
	});
	


Save your file.


	9) Clean and build your code package

At this point, all your content is set, you need only to clean your package and build it.

At your package's directory `C:\>PackageProviderDemo\packages\local\packagecode>`

Type: `sencha ant clean`

then type:

Type: `sencha package build`


	10) Create a sample app and use both theme and code package

We will now add a sample add in our workspace and use both package theme and package code locally from the workspace.

This is a good test to ensure they are working as expected.

	1) generate an app
	2) delete the sass folder content
	3) delete most of the content from app/view/main
	4) replace content of Main.js
	5) add a reference to require "PackageCode.*" to app.js
	6) add references to include both packagetheme and packagecode in app.json
	7) Build the app and test it.

Here are the steps:
	
	1) generate an app
At the root of your Workspace, type the following to generate a classic version of an ExtJS app:

`sencha -sdk ext generate app Demo demo --classic`

This will generate a scaffolding of an app with lots of stuff.

We will throw out a lot of the code in there for the sake of simplicity.


	2) delete the sass folder content

In the demo directory you will see many folders, locate the `sass` folder.

Delete all of its content.


	3) delete most of the content from app/view/main

Then in the demo folder you will locate the `app` folder

inside of `app` you will find a `view/main` folder(s)

Delete all files inside of `view/main` except for `Main.js`, this one keep it.

	4) replace content of Main.js
	

Replace the content of `Main.js` with the following:
	

	Ext.define('Demo.view.main.Main', {
	
		extend: 'PackageCode.panel.Panel',
		
		xtype: 'app-main',
		
		requires: [
		
			'Ext.plugin.Viewport',
			
			'Ext.window.MessageBox'
			
		],
		
		bodyPadding: 20,
		
		html: "Test"
		
	});
	

Save the file.

Note that we are extending the `Main` class with `PackageCode.panel.Panel`.

	5) add a reference to require "PackageCode.*" to app.js

At the root of the `demo` folder is the file `app.js`

Ensure you have the following config added: `"PackageCode.*"`

It will more than likely look like this when you are done:

    requires: [
        'Demo.view.main.Main',
        "PackageCode.*"
    ],

Save the file.


	6) add references to include both packagetheme and packagecode in app.json

Now you will locate the `app.json` file and edit it as follows:

    "framework": "ext",
    "toolkit": "classic",
    "theme": "packagetheme",
    "requires": [
        "font-awesome",
        "packagecode"
    ],

You must ensure that the config above are set as you see.

Save the file.


	7) Build the app and test it.

`sencha app clean`

`sencha app build`

Then you can open the app using a web server.


Note: If you want to use Sencha's web server, just open a cmd prompt window and navigate to the Workspace directory of this project.

Type: `sencha web -port 8090 start`

Once this is running, use a browser and type `http://localhost:8090`

Click the `demo` folder and you should see your sample app.

Expected Result:  You should have a green title background with white letter and the panel should be blue with white letter.

The title will be "My Default Panel" and the body of the text will be "Test".

If all of this works, then we can move on to Part 2.



## Part 2 - Setting up the theme package and code package to be used for "local" repo

Based on the efforts of Part 1, we will now proceed to the next step which is to ensure that these packages are available for `local` use.

As you saw in Part 1, the packages are available to the app which are all located within the same workspace.

For packages to be made available `locally` to any workspace, these packages must have their references and content added to the `\Sencha\Cmd\repo\pkgs` directory.

Once available, any app from any workspace in your machine will have access to these packages.


Note: In Part 1, both packages have their `creator` config set to `demo`, this is a crucial step towards setting up a repo.

	1) Initialize a repo

Type: `sencha package repo init -name "demo" -email "your_email@provider.com"`

	2) Verify the reference in sencha.cfg

Go and check your `Sencha\Cmd\repo\.sencha\repo\sencha.cfg` file to ensure there are references to `demo`

	3) Add each package to your local repo

Go to your workspace's `build` folder and for each of the package, go to that package and add the `pkg` file.

   a) under `build\packagecode` directory
      type: `sencha package add packagecode.pkg`
      
   b) under `build\packagetheme` directory
      type: `sencha package add packagetheme.pkg`
      

	4) Verify your packages are added to the local repo
      
Under your `Sencha\Cmd\repo\pkgs` directory you should see at least

   - packagecode <-folder
   - packagetheme <- folder
   - catalog.json
   - cert.json
   
   there may be a `.meta` and `ext` folder too.

	5) Verify that sencha can report a list of packages

   At the command prompt type: `sencha package list`

   You will expect a bunch of packages, including the 2 packages you added.


Note: At this point, any project on your system, regardless of Workspace will be able to use these new `local` repo, simply by adding references.


## Part 3 - Setting Up the packages to be distributed via "remote" and use these packages remotely

Based on the efforts of both Part 1 and Part 2, we can now finalize this demo and ensure we can set up a content delivery network (CDN) so that our packages can be made available via the web.

This is where 2 PCs are involved.

`PC 1` - This will upload the files to a web server

`PC 2` - This will set up a remote repo, create a project and use the packages via remote


	1) Upload the files to the web server.

If you recall this:

Under your `Sencha\Cmd\repo\pkgs` directory you should see at least

   - packagecode <-folder
   - packagetheme <- folder
   - catalog.json
   - cert.json
   
From `PC 1`, You need to upload those files to your web server.


	2) Sets up a PC for remote access to our Sencha repo

You will be using `PC 2` for this.

At the command prompt type the following: `sencha package repo add "demo" <http://valid address where your files are loaded>`

Once this is done, create a sample workspace and app and just use the same exact instruction on adding the references to these packages in `app.json`. Sencha Cmd will automatically download them when you do a `sencha app clean/sencha app build`.



## References

	http://docs.sencha.com/cmd/guides/cmd_packages/cmd_packages.html
	http://docs.sencha.com/cmd/guides/cmd_packages/cmd_creating_packages.html
	http://docs.sencha.com/cmd/guides/advanced_cmd/cmd_advanced.html
	http://docs.sencha.com/cmd/guides/advanced_cmd/cmd_reference.html


