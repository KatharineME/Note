+++
date = "2020-10-15"
title = "App Packaging"
slug = "app_packaging"
tags = []
categories = []
+++

## What is a .dylib?

If you are building an app, I'm sorry but you cannot escape learning about .dylibs.

dylib = dynamic library.

The goal of dynamic libraries is to reduce the launch time and memory use of an app.

A dynamic library can be directly contrasted with a static library. A static library is a library that is copied into an app's executable file. The upsides of using static libraries are: 1) The app becomes robust and less likely to break, and 2) the app is faster at runtime because all the dependent libraries have already been compiled. The downsides are: 1) the app's executable becomes large, and 2) when a static library is updated, the app executable needs to be rebuilt with the new version of the static library.

Dynamic libraries are separate files and do not become part of the app's executable file. Instead the app carries dynamic library references. When the app is run, a link is established between the app and the actual dynamic libraries. The upsides are 1) the app's executable is smaller, and 2) apps automatically use and benefit from any upates to the dyanmic libraries. The downside is that the app is less robust.

A dynamic library is called and activated based on its software language and operating system (OS).

More Reading

- https://medium.com/@StueyGK/static-libraries-vs-dynamic-libraries-af78f0b5f1e4
- https://developer.apple.com/library/archive/documentation/DeveloperTools/Conceptual/DynamicLibraries/100-Articles/OverviewOfDynamicLibraries.html

## How to Distribute an App

Assuming that the app is already made and is self-contained, you can either distribute it via the Mac App Store, or outside of it. If outside the MacApp Store, you need a way to help users install the app. To do this, you can use a .dmg or a .pkg.

#### What is a .dmg?

DMG stands for disk image file and is used to install software but can contain any type of file. A dmg doesn't know whats inside of it. For example, you could make a dmg for a single text file, and that dmg would place the text file in the Applications folder for example. A .dmg is usually used when a simple copy to the Applications folder is needed for example.

#### What is a .pkg?

PKG are macOS installation packages which contain installer scripts and compressed installation files that are used to install Mac software. A .pkg knows the files iside of it. A .pkg has instructions for how the software it carries should be installed. When you click on a .pkg, the os will follow the instructions. A .pkg can install files in different locations. Generally, people will use a .pkg instead of a .dmg when they need to include installer scripts.

#### What is a Flat Package?

"Flat Package" is usually how people refer to a .pkg which installs some kind of application. Flat packages are xar archives. You can use `pkgutil --expand` to open the content of a flat package, and `pkgutil --flatten` to recreate the .pkg. Flat packages can be a product archive or a component package.

A product archive is a meta-package containing one or more component packages with an XML file specifying the installer behavior. The command line tool `productbuild` can create product archives.

A component package is an installer that installs one component. It can be included as a part of a product archive, or can be an installer on its own.

More Reading

- https://matthew-brett.github.io/docosx/flat_packages.html
