# Patch Instructions

Fork `https://github.com/waldur/waldur-helm`.

Create branch for GitHub pages:

```shell
git checkout --orphan gh-pages
git reset --hard
git commit --allow-empty -m "Init gh-pages"
git push origin gh-pages
```

Create initial branch from the earliest upstream tag we want:

```shell
# Add upstream remote (once)
git remote add upstream https://github.com/waldur/waldur-helm.git
git fetch upstream --tags

# Create a branch from the earliest upstream tag you want
git checkout -b release/8.0.6 8.0.6

# Apply customizations
# .github/workflows/release-charts.yml
# rmq-values.yaml
# waldur/values.yaml

git commit -m 'Fix erroneous network policy ingress setting'
git push origin release/8.0.6
```

Release patched upstream:

```shell
# No merge or rebase needed since they have the same commits
git checkout -b release/current release/8.0.6
git push origin release/current
```

## Releasing a new upstream tag

```shell
git fetch upstream --tags

# Create new release branch from a new upstream tag
git checkout -b release/8.0.7 8.0.7

# Find the commit hash of the patches:
git log release/8.0.6 --oneline

git cherry-pick <commit-hash>

# Update release branch
git checkout release/current

# reset --hard
# -> moves release to point at your new patch branch,
#      replacing the old upstream version
git reset --hard release/8.0.7

git push origin release/current --force-with-lease
```
