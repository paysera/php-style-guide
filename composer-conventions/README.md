Composer Conventions
=====

## Semantic versioning

We always use semantic versioning on library repositories.

> **Why?** When backward incompatible change is made in library, we have two options: 1) update all client side projects with new library version 2) every time when we update library check for previous backward incompatible changes, not related to currently added feature. As it happens, sometimes none of these 2 takes place.

## Backward incompatible changes

If we make backward incompatible change, we always provide some basic information about what was changed in `CHANGELOG.md` or `UPGRADE.md` file.

## Releases

We tag each and every commit (except instant bug-fixes or commits before separate merge commits).

1. Right before tagging, we make sure we're on master branch and always run `git tag --sort=v:refname` to see current tags.
2. Before tagging, we see available branches on repository as version can be created by branch alias, not only by tags.
3. We bump MAJOR, MINOR or PATCH version by one from latest version available (or parent commit if there are few active branches).

    3.1. If we make backward incompatible change, we bump the MAJOR version.

    3.2. If we add new feature (any new arguments, method calls, classes etc.), we bump MINOR version.

    3.3. If we do not change API of library in any way, just change the internals (usually bug-fixes), we bump PATCH version.

We use annotated tags so that time and author would be present. Example command, `1.2.3` taken as example tag:

```
git tag -a -m "" 1.2.3
```

`-m ""` sets message to the tag, but as all commits are already with messages, we can leave it empty.
 
After tagging we need to call `git push origin (tag)` - this pushes our tags to origin. We only do this after we've landed our changes.
 
After pushing, we need to manually update library in toran: go to toran, select "Private repositories", find yours and press "Update".
## Reviewing tagging policy

In differential revision we always comment on intended tagging policy - `MAJOR`, `MINOR` or `PATCH`. As this can change (wrong initial policy denied by review or changed after additional commits in review), we provide this information in a summary or in a comment, not in commit message (title of revision).

Reviewer must reject a diff, if `MAJOR` bump is needed and no information is provided about backward incompatible changes.

## Initial library development

Until library is stable, versions can change quite often. In this case we still use tags and semantic versioning, but use `0` as MAJOR version. In this case any backward incompatible change can be made in any MINOR version bump. If API of library is quite stable and is not to change much, we use `1` as a MAJOR version initially.

## Constraints on library versions

We use as generic constraints as possible for required libraries. This avoids updating some library graph just because of some conservative constraint, and not because it is really needed.

For example, we have `lib-a@1.0.0` and `lib-b@1.0.0`, which depends on `lib-a: ^1.0`. If we roll out MAJOR release for `lib-a` (aka `lib-a@2.0.0`), at some time we need to modify constraint on `lib-b`:

1. If `lib-b` works with both `lib-a@1.0.0` and `lib-a@2.0.0` (change did not affect this library), we use `lib-a: >=1.0.0,<3.0`. This allows to use both `1.0` and `2.0` with our new version of `lib-b`.
2. If `lib-b` works only with `lib-a@2.0.0` and not with `lib-a@1.0.0`, we modify constraint as usual to `lib-a: ^2.0.0`.
3. If `lib-b` does not work with `lib-a@2.0.0`, we can either leave constraint to `^1.0` or update code and use (1) or (2) depending on compatibility with `1.0` version. Latter is recommended, as this allows to use newest possible versions (sooner or later we will need to update them).

Version in `lib-b` in any of these cases is not required to make a MAJOR bump - if API of `lib-b` is left the same, we can make PATCH update. We can even entirely drop out `lib-a` and use some alternative library, as long as API leaves the same.

This means that if `app-x` (or any other library) uses functions or classes from `lib-a`, it must require it in `composer.json`. If it just depends on `lib-b`, it does not care about `lib-a` versions or even if it is installed at all.

Constraints on vendors
=====

If we require vendor library, we use constraint depending on versioning strategy of that library. If it states, that it follows semantic versioning (and we believe it does), we use something like `^1.2.3` - this is added by default if we run `composer require vendor/library`. If it has some other strategy, we must correct the constraint so that we would not get unexpected backward incompatible changes. For example, Symfony components did break compatibility on minor releases (at least in 2.X ones). In that case, we must not use `^` in constraint.

Basically, we must always assume that `composer update` can be run at any time and everything should still work as expected.

Due to the same reason, we never require `dev-master`. If vendor library has no versions defined, we require specific commit:

```
"vendor/package": "dev-master#8e45af93e4dc39c22effdc4cd6e5529e25c31735"
```
 
Making minor/patch release on previous version
=====
 
Usually we try to have single branch in origin. This way we only add features to the latest code and upgrade our software to use the newest versions of our libraries.
 
In some cases, it might be needed to update/change code in some previous version of the library.
 
Let's say, that library has versions 1.0, 2.0, 3.0, 3.1. We need to add change to 1.0 and make 1.1 version. In that case:
 
1. Checkout tag we want to change (1.0 in example). Command: `git checkout tags/<tag_name> -b <branch_name>`
2. Allow Dangerous changes for that library in Phabricator
3. Push immediately with no changes to create new branch. Command: `git push --set-upstream <remote-name> <local-branch-name>`. Remote name should be `1.x` or `v1` in this example.
4. Make changes, arc diff...
5. Add git tag
 
This creates additional branch so we can add fixes / new features and release 1.1 without changing 3.x versions.

