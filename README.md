# CircleCI "cimg" Convenience Images Overview

The CircleCI `cimg` Convenience Images are a suite of Docker images that are intended to replace the legacy Convenience Images.
The legacy images live in [this repository](https://github.com/circleci/circleci-images) and use the Docker Hub orgname `circleci`.
The new images live in this GitHub org (`CircleCI-Public`) and the repository names are prefixed with `cimg-`.
The Docker Hub orgname for the new images is `cimg`.

Each Docker image has a repository where the image is built and its `Dockerfile` is located.
This GitHub org is maintained by the CircleCI Community & Partner Engineering Team.

This repository serves as an overview of CircleCI's revamped Docker convenience images pilot project. This project is intended to improve the user experience for CircleCI convenience images and solve a number of underlying problems in the original images' codebase.

## Status

Since there are many images, they are in different stages of their life cycle.
This table will show where each image is and how "ready" they are to replace their predecessor.

| Name | Image | Release Life Cycle | GitHub | Docker Hub | Notes |
| --- | --- | --- | --- | --- | --- |
| Ubuntu Base | `cimg/base` |  **public beta** | [link](https://github.com/CircleCI-Public/cimg-base) | [link](https://hub.docker.com/r/cimg/base) | new image |
| Android | `cimg/android` |  **alpha** | [link](https://github.com/CircleCI-Public/cimg-android) | [link](https://hub.docker.com/r/cimg/alpha) | Replaces `circleci/android` |
| Clojure | `cimg/clojure` |  **public beta** | [link](https://github.com/CircleCI-Public/cimg-clojure) | [link](https://hub.docker.com/r/cimg/clojure) | Replaces `circleci/clojure` |
| Elixir | `cimg/elixir` |  **private beta** | [link](https://github.com/CircleCI-Public/cimg-elixir) | [link](https://hub.docker.com/r/cimg/elixir) | Replaces `circleci/elixir` |
| Go | `cimg/go` |  **public beta** | [link](https://github.com/CircleCI-Public/cimg-go) | [link](https://hub.docker.com/r/cimg/go) | Replaces `circleci/golang` |
| Node.js | `cimg/node` |  **public beta** | [link](https://github.com/CircleCI-Public/cimg-node) | [link](https://hub.docker.com/r/cimg/node) | Replaces `circleci/node` |
| OpenJDK | `cimg/openjdk` |  **public beta** | [link](https://github.com/CircleCI-Public/cimg-openjdk) | [link](https://hub.docker.com/r/cimg/openjdk) | Replaces `circleci/openjdk` |
| PHP | `cimg/php` |  **public beta** | [link](https://github.com/CircleCI-Public/cimg-php) | [link](https://hub.docker.com/r/cimg/php) | Replaces `circleci/php` |
| Python | `cimg/python` |  **public beta** | [link](https://github.com/CircleCI-Public/cimg-python) | [link](https://hub.docker.com/r/cimg/python) | Replaces `circleci/python` |
| Ruby | `cimg/ruby` |  **public beta** | [link](https://github.com/CircleCI-Public/cimg-ruby) | [link](https://hub.docker.com/r/cimg/ruby) | Replaces `circleci/ruby` |
| Rust | `cimg/rust` |  **public beta** | [link](https://github.com/CircleCI-Public/cimg-rust) | [link](https://hub.docker.com/r/cimg/rust) | Replaces `circleci/rust` |

**Release Life Cycle:**
- alpha - image is recently created and still in the prototyping phase. Will have big changes and may even be deleted.
- private beta - image is tested on a 1 on 1 basis with specific people and teams to gather feedback. Breaking changes likely.
- public beta - image can be used by anyone but please provide feedback if you have it, breaking changes less likely.
- stable - image is considered production ready. Breaking changes are rare and will be communicated first.


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
  For example, to build Python v3.6.2, you would run: `./shared/generate-images.sh 3.6.2`.
  The result of running this script is that directories for each major.minor version of the software is created, and `build-images.sh` is created.
  Running this directly won't be necessary for regular version updates.
  Instead, you can run the `release.sh` script.

  `shared/release.sh` - This script should be run from the root of the repository, on the `master` branch.
  This script automates most of the work for releasing a new image.
  It will create a new branch for the version, run `generate-images.sh` for you, add all the files to git, create a commit, and push the branch to GitHub.
  All you'd need to do afterward is open the PR itself.
  Running this script is similar to running `generate-images.sh` as they use the same parameters.
  For example, to build Python v3.6.2, you would run: `./shared/release.sh 3.6.2`.

  Both scripts support more advanced versioning support for your images.
  See the [Version Support](#version-support-version-groups) section for more information.


## Version Support / Version Groups

In the examples so far, we see that the scripts `gen-dockerfiles.sh` and `release.sh` can be passed a version to build a release.
The `cimg-shared` repository actually has more powerful support than that.
Let's breakdown what's supported.

1. As we know, a version can be passed to the scripts.
More specifically, the version passed should be a full SemVer version number (major.minor.patch) or a partial SemVer (major.minor).
With either format, an optional letter 'v' can prefix the version.
Here are valid versions: `1.14`, `1.13.6`, or `v2.10.0`.
1. Multiple version numbers can be passed.
Each one should be separated by a space.
The build system will build these images from left to right so older versions should be listed first and newer versions later.
1. Each version passed to the script is actually called a "version group".
This is because in addition to the actual version number, a version alias or other parameter can be passed.
    - An alias is a string that can be used instead of a version number.
    This is common for projects such as Ubuntu where Ubuntu 20.04 could be called "focal" or Ubuntu 18.04 could be called "bionic".
    Another common case is for Node.js which always runs two series of code at the same time.
    They always have a "current" version as well as a "lts" version.
    An alias can be passed right after the version with an equal sign (=).
    For example: `14.0.0=current`, `v12.16.3=lts`, or `20.04=focal`.
    - An additional parameter could be provided as well.
    Some images might want to be passed a SHA hash, some a special download URL, etc.
    This can be passed to the scripts either after the version or after an alias with a hash (#).
    For example: `14.0.0#847r84uhuf4848`, `v12.16.3#https://example.com/download/12.16.3.tar.gz`, or `20.04#server-edition`.
    - A version, alias, and parameter together are called a version group.
    Only the version number is required in a version group, the other two are optional.
    As before, each version group is separated by a space.
1. Here's more examples of what's valid to pass to `gen-dockerfiles.sh` or `release.sh`:
    ```
    1.13.1 v1.14.2
    v1.13.1#sha256abcfabdbc674bcg
    v13.0.1=lts
    v20.04
    v8.0.252=lts=https://example.com/download/item.tar-gz
    ```

### Extending a Base Image

All of our Docker images extend a base image.
The majority of our images will extend the CircleCI Base Image `cimg/base`.
This base image, and thus most of the CircleCI Convenience Images, will be utilizing Ubuntu.
The base image will itself be based on the latest Ubuntu LTS release, 2 months after it's published.
For example, our base image will default to Ubuntu 20.04 in July 2020, and Ubuntu 22.04 in July 2022.
LTSs will be supported for 3 years total, about 2 years as the default and about 1 year once it's no longer the default.

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
