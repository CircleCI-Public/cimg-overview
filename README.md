# CircleCI "Prototype" Convenience Images Overview

The `cci-images` GitHub organization contains all of the official `cimg` namespaced Docker images.
Each Docker image has a repository where the image is built and its `Dockerfile` is located.
This GitHub org is maintained by the CircleCI Community & Partner Engineering Team.

This repository serves as an overview of CircleCI's revamped Docker convenience images pilot project. This project is intended to improve the user experience for CircleCI convenience images and solve a number of underlying problems in the original images' codebase.

## Structure

### Image Repository

An `ls -lah` of a typical image repository will show the following:

```
drwxr-xr-x 4.0K Jul 10 13:35 .
drwxrwxr-x 4.0K Jul 10 11:50 ..
-rwxr-xr-x   20 Jul 10 12:46 build-images.sh
drwxr-xr-x 4.0K Jul 10 13:35 .circleci
-rw-r--r--  242 Jul 10 13:35 Dockerfile.template
drwxr-xr-x 4.0K Jul 10 13:36 .git
-rw-r--r--   90 Jul 10 11:40 .gitmodules
-rw-r--r-- 1.1K Jul 10 12:46 LICENSE
-rw-r--r--   67 Jul 10 12:46 manifest
-rw-r--r-- 1.2K Jul 10 13:34 README.md
drwxr-xr-x 4.0K Jul 10 11:33 shared
```

We'll walk through the files one by one.

`build-images.sh` - This is a Bash script that contains the `docker build` command(s) that will build the image(s).
It also is designed to build an image with the main Docker tag and any alias tags it may need.
This file is generated from `shared/generate-images.sh` (covered later) and should not be edited by hand.

`.circleci` - This is a directory containing the CircleCI configuration file.

`Dockerfile.template` - A template to create a Dockerfile.
This contains the `%%MAIN_VERSION%%` and `%%MAIN_SHA%%` variables that will be replaced by `shared/generate-images.sh` script (covered later).
These variables are the main software version and SHA.
For example, the Ruby version or Go version.
This file is the guts of what makes an image.
The middle of this file should have RUN/CP statements that configure the container to do whatever the image needs to do.
Typically, this is installing the language, toolchain, and any relevant package managers and CI tools specific to this image.

`.git`, `.gitmodules` - Git related files we won't touch directly.

`LICENSE` - The source code license file.
All of our images at the moment will be using the MIT license.

`manifest` - This small but important file holds key information about the image.
It holds the slug name of the repository, the slug name of the parent image this image is based off of, and what variants from the parent image it will use.

`README.md` - The readme file should be helpful for people to know what the image is, how it works, and how to contribute.
Outside of some image specific information, it should link to this repo for deeper understanding of how everything works.

`shared`- This directory is a submodule pointing to the `cimg-shared` repo.
This repo can be found [here](https://github.com/CircleCI-Public/cimg-shared).

  `shared/generate-images.sh` - This Bash script uses the information passed as commandline arguments, and the `manifest` file, to turn `Dockerfile.template` into multiple, buildable, Dockerfiles.
  To you, from the root of the repo, you run it and pass the version of the software to build, and the SHA if you have one.
  For example, to build Python v3.6.2, you would run: `./shared/generate-images.sh 3.6.2 fake-sha`.
  The result of running this script is that directories for each major.minor version of the software is created, and `build-images.sh` is created.
  Running this directly won't be necessary for regular version updates.
  Instead, you can run the `release.sh` script.

  `shared/release.sh` - This script should be run from the root of the repository, on the `master` branch.
  This script automates most of the work for releasing a new image.
  It will create a new branch for the version, run `generate-images.sh` for you, add all the files to git, create a commit, and push the branch to GitHub.
  All you'd need to do afterward is open the PR itself.
  Running this script is similar to running `generate-images.sh` as they use the same parameters.
  For example, to build Python v3.6.2, you would run: `./shared/release.sh 3.6.2 fake-sha`.


## Contributing

Each image has its own repository.
Please find the repository for the image in question, and then open a GitHub Issue or Pull Requests in that repository.
GitHub Issues shouldn't be used for general image support, see [resources](#resources).

### Creating New Repositories/Orbs

1. The CPE Team should be added as admin.
1. Force status checks should be enabled for `master`.
1. Protect from pushing to `master` directly.
1. Turn off Wiki/Projects for the repo.

## Resources

This section needs to be built out.
Links to improved/new Image Docs as well as a second in Discuss needs to be created.

See [the `cimg` topic tag on the CircleCI-Public organization](https://github.com/search?q=topic%3Acimg+org%3ACircleCI-Public&type=Repositories) for all repositories associated with new images work. The repositories under this topic tag consist of the following:

- Documentation and shared resources ([`cimg-overview`](https://github.com/CircleCI-Public/cimg-overview), [`cimg-docs`](https://github.com/CircleCI-Public/cimg-docs), [`cimg-shared`](https://github.com/CircleCI-Public/cimg-shared));
- A single base image ([`cimg-base`](https://github.com/CircleCI-Public/cimg-base));
- A repository for each language/tool image covered by the current convenience images fleet (so far, only [`cimg-golang`](https://github.com/CircleCI-Public/cimg-golang) and [`cimg-ruby`](https://github.com/CircleCI-Public/cimg-ruby) exist)


## Creating a New Image

The easiest way to create a new CircleCI Convenience Image is by cloning the `cimg-template` repository.
Don't fork.
Then change the Git remote to your new, empty repo.
This repo can be found [here](https://github.com/CircleCI-Public/cimg-template).
You'll want to start customising your repo for the image you're building.
The first step is to replace instances of `<<IMAGE_NAME>>` with the slug name of the image.
For example, if we were to setup the Python image, we would do the following:

```
sed -i 's/<<IMAGE_NAME>>/python/g' README.md manifest .circleci/config.yml
```
