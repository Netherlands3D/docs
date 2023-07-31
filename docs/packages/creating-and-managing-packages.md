Creating and managing Packages
==============================

Netherlands3D is a framework where you, as developer, can pick the functionality that you need from a series of 
packages. This guide will empower you to develop your own packages by providing step-by-step instructions how
you can create your own package, release it and last, but not least, publish it to [OpenUPM](https://openupm.com).

## Creating a new package

When creating a new package, it is recommended to first create it as an embedded package in the 
[Netherlands3D twin project](https://github.com/Netherlands3D/twin). This will help to prototype and experiment until 
the point where it is stable enough to be distributed.

This can be done using the following steps:

1. Clone the Netherlands3D Twin project, located at https://github.com/Netherlands3D/twin.

2. Make sure Unity is not open while performing the following steps; it generally tends to crash when it updates while setting up a package.

3. Create a directory for your [embedded package](https://docs.unity3d.com/Manual/CustomPackages.html#EmbedMe) in the 
   `Packages` folder.

   > The directory name *MUST* follow the structure `eu.netherlands3d.[name]` where `name` is in lower-case [kebab-case](https://www.pluralsight.com/blog/software-development/programming-naming-conventions-explained#kebab-case).

4. Copy the template files, located at https://gist.github.com/mvriel/8a8251b492d9d8f742da16667c49e412, and fill in the placeholders.

   > #### Placeholders
   > The template files in the Gist above uses various placeholders -recognized as they are between brackets-, this
   > table provides an overview of them and their meaning.
   >
   > | Name              | Explanation                                                                                                                                                                             | Example                                                                                                    |
   > |-------------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|------------------------------------------------------------------------------------------------------------|
   > | NAME              | The name of the repository, this is usually the DISPLAY_NAME in PascalCase.                                                                                                             | PeriodicSnapshots                                                                                          |
   > | PACKAGE_NAME      | The package name as known in the package manager and/or package.json; always starts with `eu.netherlands3d.` and followed by the display name -or similar- in kebab-case                | eu.netherlands3d.periodic-snapshots                                                                        |
   > | DISPLAY_NAME      | The human-readable name of a package, shown in the README or in the Package Manager                                                                                                     | Periodic Snapshots                                                                                         |
   > | REPOSITORY_NAME   | Alias of NAME, used to make explicit that we want the repository name there.                                                                                                            | PeriodicSnapshots                                                                                          |
   > | VERSION           | The version number in the package.json, according to SemVer. **Important**: contrary to the git tag, this does not start with a letter `v`                                              | 1.0.1                                                                                                      |
   > | DESCRIPTION       | A single-line short description describing the package, can also be used on Github as the description for the repository                                                                | This package provides the means to do take a series of snapshots for specific moments throughout the year. |
   > | LONG_DESCRIPTION  | The description of the package detailing what it is used for.                                                                                                                           |                                                                                                            |
   > | USAGE_INFORMATION | Documentation how to use this package.                                                                                                                                                  | *See existing packages for examples*                                                                       |
   > | NOTE              | One or more entries in the CHANGELOG -according to [Keep A Changelog](http://keepachangelog.com/en/1.0.0/)- describing what has been added, changed, removed or deprecated in a package |                                                                                                            |

5. Set up the package according to the following recommended directory structure: https://docs.unity3d.com/Manual/cus-layout.html

6. Start Unity and allow for it to install the new package, you should see your new package in the "Installed Packages" section of the Package Manager with the tag `Custom` behind the name.

7. When you want to add Scripts to this package: Make sure you have created the folder `Runtime\Scripts` as a location to store them and add an Assembly Definition with the name `{PACKAGE_NAME}.Runtime`. 

<!---
## Promoting an embedded package

## Changing a package
-->

## Releasing a package

When you want to release a new version of a package you generally go through the following steps:

1. Go to the repository of your package <!--- , if there is none: see the chapter on [Promoting an embedded package](#promoting-an-embedded-package) -->

2. Ensure the version number in the package.json is updated

3. Check the CHANGELOG.md; does it contain all changes since the last version?

4. Ensure any changes in the above are in the main branch

5. Go to "Releases" on Github (`https://github.com/Netherlands3D/{NAME}/releases`) and

   1. Draft a new release
      ![Draft new release](../imgs/packages/draft-release.png)
   2. Click on "Choose a tag"
   3. Enter the version number from the package.json with a preceding letter `v` -example: `v1.0.1`-.
   4. Click on the option "Create new tag: v{VERSION} on publish"
   5. (optional) Add a release title and description
   6. Click on the button "Publish Release"

Once a new release/tag has been made, your new release of your package is all set! If it has already been 
published on [OpenUPM](https://openupm.com) before, no further action is needed. OpenUPM will automatically pick
up on the new tag and make the new version available.

When the package has not been published on OpenUPM yet, now is a good time to do it.

<!---
## Publishing a package on OpenUPM
-->
