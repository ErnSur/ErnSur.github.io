---
title: "Maintaining and Delivering a Unity Package 📦"
date: 2022-07-16T21:39:28+02:00
draft: true
---

Whether you call it a library or a package, it’s a common practice to bundle a bunch of code into an entity that you might later reuse on many different projects. To distribute it, you usually use package managers like npm, pip, or NuGet.

## What about Unity?

Even though NuGet manages C# code, Unity was not designed to be compatible with it. NuGet also delivers DLLs, which also limit development capabilities in the Unity environment. This is because Unity Editor and its libraries API evolves fast, and without conditional compilation, it becomes harder to code against many different versions of the editor and its modules. Another difference between the regular .NET package and Unity is that the Unity package may depend on assets other than the code.

# Current packaging solution

Currently, the most common way to share your code and assets in the Unity community is with Unity’s archive format “.unitypackage”. For the sake of simplicity, I will call .unitypackage files “asset packages”, same as in the [Unity manual](https://docs.unity3d.com/Manual/AssetPackages.html).

If you are a code library maintainer and want your users to be happy, there are some common aspects you will want your package to have:

-   Easy to import
-   Easy to update
-   Easy to maintain
-   Easy to remove
-   Hard to break

## Problems with asset packages

The asset package itself is simply an archive, and is not connected to any package management system. Importing it just adds new files and overrides old ones. That’s why delivering library updates with asset packages presents certain problems:

## Hard to update ❌

Let’s say that you created a new asset package version. Some of the changes include removed or relocated files. The user is now importing this new package but already has an older version of it in the project. Files that were previously removed will now be present again. This is especially important if you are working with special folders in Unity like Resources or Editor: if you want to move existing assets in or out of them, you’re out of luck. You will either have to ask the user to delete the old package by him/herself or you will need to write some code to delete those files automatically — which is not an easy task, considering its mutable state.

## Hard to remove and Easy to break ❌

- Unity Projects consist of many different folders, but when you open up Unity Editor you can only see two of them inside the Project Window: Assets and Packages.  
- The assets folder is the main working directory and there is no common standard on how developers and artists organize it. They will create, delete and move files constantly.  
- Most of the Asset Packages import files into this directory. Some Assets Packages can import more than one folder into this root directory, scattering the library files across many different folders. Import many different SDKs, and you will soon find that those changes to the project are hard to track.

## Better alternative

Starting with Unity 2018 we can use Unity Package Manager or UPM, a system that can download, install, and update packages that are treated as immutable dependencies of your project.

Pros of UPM format package:

-   Easy to manage packages used in the project
-   Package metadata is easily accessible: version, author, source, samples, etc.
-   Dependency resolution

There are already good resources on how to create a UPM package and even set up a CD for it.

[Simple Guide to Unity Package Management | by Paul Harwood | Runic Software | Medium](https://medium.com/runic-software/simple-guide-to-unity-package-management-4aea43d1baf7)

Let’s focus on your distribution options based on our use case:

# Open source package, available for everyone:

## Git repository

This is the simplest one, you just need to create a public repository where you’ll be developing the package. Additionally, you can add your package to [OpenUPM.com](https://openupm.com/), an Open Source Unity Package Registry.

Your users will be able to add and update your package using either a git URL or OpenUPM CLI tool.

Pros:

-   Easy to setup
-   Easy to add, update and remove

Cons:

-   Limited dependency resolution
-   Package needs to be public

## Scoped Registry

> _Scoped Registries allow Unity to communicate the location of any custom package registry server to the Package Manager so that the user has access to several collections of packages at the same time._

You can set up a server or use an existing one to publish your packages to. Later users may add this server as a scoped registry to their Unity Project. This allows the user to browse, install, and update every package hosted on the server with dependency resolution, as long as the dependencies are in the scope of user registries.

An example of such a registry is the [OpenUPM](https://openupm.com/) project.

Pros:

-   Browse available packages inside Unity UI (with limitations)
-   Dependency resolution

Cons:

-   More set up on the user and distributor side.

[Run your own Unity Package Server! | by Markus X. Hofer | Medium](https://medium.com/@markushofer/run-your-own-unity-package-server-b4fe9995704e)

## Tarballs

The tarball is also an archive file, but when imported into Unity its content is treated as immutable.

**When imported into Unity:**

-   Tarball becomes a project asset, unlike Asset Package which is immediately extracted into a project while the package itself is not part of it.
-   Extracted files from tarball are kept outside of Assets or Packages directories. They are stored in the Library folder.

Pros:

-   Immutable
-   Doesn’t depend on the server access
-   Relatively easy to add, update and remove

Cons:

-   Unity 2018 has no UI for importing tarballs
-   Limited dependency resolution

## Asset Package with UPM package

There is also a concept of an embedded UPM package. This is a package that is stored locally in the “Packages” directory. This is the case, for example, when you are developing a UPM package. What you could do is simply export the package directory as an asset package and share it this way.

![](https://miro.medium.com/max/700/1*z-IaUBKVBnOM5mtGK7lfGg.png)

Package exported from Packages folder with `[AssetDatabase.ExportPackage](https://docs.unity3d.com/ScriptReference/AssetDatabase.ExportPackage.html)`

This solution sadly has only one small advantage which is the package being outside of the Assets folder.

Pros:

-   Outside of Assets folder

Cons:

-   All of the cons of Asset Package

# Closed source package, private/internal use

If you work in a company and share your internal libraries with your co-workers more setup is needed.

## Simplest option: private repositories

You can host your package in a private repository and add it to the project with a git URL. The limitation of this solution is that UPM won’t ask you for credentials to authenticate your access. The simplest solution, in this case, is to use SSH URLs. The [Unity manual](https://docs.unity3d.com/Manual/upm-errors.html#prompts-disabled) has more info regarding this problem.

## Private Scoped Registry

This solution would be viable for companies where there are many internal packages and an administrator that can manage this setup. Unfortunately for now (2021), this solution is more of an MVP feature.

Pros:

-   Browse available packages from Editor UI
-   Very easy to update packages
-   Dependency resolution

Cons:

-   Available only for Unity Editor 2019.3.4f1+
-   Requires convoluted setup for registry and each user

## Tarballs

If you want to share your package with selected users but don’t want them to access your repository and set up the ssh key you can use tarballs.

# My Solution ✔️

I work at [Sayollo](https://www.sayollo.com/), where I develop a Unity SDK that enables developers to integrate eCommerce and add ads into the 3D space of their games. Because of it, I have some limitations in what I can do. For example, I need to distribute my package to authorized users only, so I need to narrow my options to closed-source ones.  
I also can’t expect my users to go over an elaborate setup process on each system just to access my package, so I have to rule out the private repository and scoped registry options.  
This leaves me with tarballs. Unfortunately, this option also has some big problems for my use case.

My package has some dependencies, and one of them is also not distributed in UPM format. To allow my users to easily update this dependency by themselves using the asset store, I have to limit myself from modifying its content.  
The following reasons force me to still depend on Assets Packages, but what can I do to work with its disadvantages? I use the following design rules to allow my users to easily remove, relocate, and update my package.

**Create a package that’s immutable by design**

-   Don’t require the user to modify package content.
-   Don’t modify package content in its lifetime.
-   Store package generated assets in a separate directory.
-   Don’t depend on absolute paths to your package.

This way, when the user wants to update the old package folder, it can be deleted without worrying about lost content. They can also move the package folder if they don’t want to clutter their root directory.

## UPM Summary

UPM package is certainly a step in the right direction, but still not an ideal solution.

-   Unity 2018's package manager is missing some features, which is important when you still have users using this version.
-   Distributing a single file package when it depends on other (non-official) UPM packages is impossible without adding a new scoped registry beforehand or third-party solutions.
-   [Unity guidelines](https://unity3d.com/legal/terms-of-service/software/package-guidelines) prevent you from utilizing the full power of the system, mainly that you cannot _add, update or modify installed Packages on behalf of the user outside the official Unity Editor UI, such as programmatically through scripts._

The last one hurts the most, obviously. The introduction of these guidelines forced Google to shut down their public registry where a user could easily manage their SDKs. But this system is still new in the Unity ecosystem and is improving from version to version, which gives us some hope for the future.