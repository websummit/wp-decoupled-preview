## Contributing ##

The best way to contribute to the development of this plugin is by participating on the GitHub project:

https://github.com/pantheon-systems/wp-decoupled-preview

Pull requests and issues are welcome!

## Workflow

Development and releases are structured around two branches, `develop` and `main`. The `develop` branch is the default branch for the repository, and is the source and destination for feature branches.

We prefer to squash commits (i.e. avoid merge PRs) from a feature branch into `develop` when merging, and to include the PR # in the commit message. PRs to `develop` should also include any relevent updates to the changelog in readme.txt. For example, if a feature constitutes a minor or major version bump, that version update should be discussed and made as part of approving and merging the feature into `develop`.

`develop` should be stable and usable, though possibly a few commits ahead of the public release on wp.org.

The `main` branch matches the latest stable release deployed to [wp.org](wp.org).

## Release Process

1. From `develop`, checkout a new branch `release_X.Y.Z`.
1. Make a release commit:
    * Drop the `-dev` from the version number in [readme.txt](./readme.txt) and [wp-decoupled-preview.php](./wp-decoupled-preview.php)
    * Update the "Latest" heading in the changelog to the new version number with the date
    * Commit these changes with the message `Release X.Y.Z`
    * Push the release branch up.
1. Open a Pull Request to merge `release_X.Y.Z` into `main`. Your PR should consist of all commits to `develop` since the last release, and one commit to update the version number. The PR name should also be `Release X.Y.Z`.
1. After all tests pass and you have received approval from a [CODEOWNER](./CODEOWNERS), merge the PR into `main`. "Rebase and merge" is preferred in this case. _Never_ squash to `main`.
1. Pull `main` locally, create a new tag (based on version number from previous steps), and push up. The tag should _only_ be the version number. It _should not_ be prefixed  `v` (i.e. `X.Y.Z`, not `vX.Y.X`).
1. Create a [new release](https://github.com/pantheon-systems/wp-decoupled-preview/releases/new) using the tag created in the previous steps, naming the release with the new version number, and targeting the tag created in the previous step. Paste the release changelog from the `Changelog` section of [the readme](readme.txt) into the body of the release, including the links to the closed issues if applicable.
1. Wait for the [_Release wp-decoupled-preview plugin to wp.org_ action](https://github.com/pantheon-systems/wp-decoupled-preview/actions/workflows/wordpress-plugin-deploy.yml) to finish deploying to the WordPress.org plugin repository. If all goes well, users with SVN commit access for that plugin will receive an emailed diff of changes.
1. Check WordPress.org: Ensure that the changes are live on [the plugin repository](https://wordpress.org/plugins/decoupled-preview/). This may take a few minutes.
1. Following the release, prepare the next dev version with the following steps:
    * `git checkout develop`
    * `git rebase main`
    * Update the version number in all locations, incrementing the version by one patch version, and add the `-dev` flag (e.g. after releasing `1.2.3`, the new verison will be `1.2.4-dev`)
    * Add a new `** Latest **` heading to the changelog
    * `git add -A .`
    * `git commit -m "Prepare X.Y.X-dev"`
    * `git push origin develop`