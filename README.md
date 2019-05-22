# `cimg` Overview

This repository serves as an overview of CircleCI's revamped Docker convenience images pilot project. This project is intended to improve the user experience for CircleCI convenience images and solve a number of underlying problems in the original images' codebase.

## Structure

See [the `cimg` topic tag on the CircleCI-Public organization](https://github.com/search?q=topic%3Acimg+org%3ACircleCI-Public&type=Repositories) for all repositories associated with new images work. The repositories under this topic tag consist of the following:

- Documentation and shared resources ([`cimg-overview`](https://github.com/CircleCI-Public/cimg-overview), [`cimg-docs`](https://github.com/CircleCI-Public/cimg-docs), [`cimg-shared`](https://github.com/CircleCI-Public/cimg-shared));
- A single base image ([`cimg-base`](https://github.com/CircleCI-Public/cimg-base));
- A repository for each language/tool image covered by the current convenience images fleet (so far, only [`cimg-golang`](https://github.com/CircleCI-Public/cimg-golang) and [`cimg-ruby`](https://github.com/CircleCI-Public/cimg-ruby) exist)

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
