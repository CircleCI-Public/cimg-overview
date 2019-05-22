# `cimg` Overview
[the `cimg` topic tag on the CircleCI-Public organization](https://github.com/search?q=topic%3Acimg+org%3ACircleCI-Public&type=Repositories) for all repositories associated with new images work:


## Structure

Each repository is an image or build tools.
The repository name should be the image's name under the `circleci` namespace.
For example, the Go image would be used as `cimg/golang` and it's repository is `github.com/cci-images/golang`.


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
