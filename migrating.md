# Migrating from Legacy to Next-Gen Images

The Next-Gen images have a lot of changes from the legacy ones.
Here's the in-image changes that would affect projects migrating from the old images to the new ones.

All

- No more `circleci` user. The default user is root.
  - This matters most for configs that are using `sudo`. Remove the use of `sudo`.
