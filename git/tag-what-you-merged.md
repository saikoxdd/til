# Check what the tag points at before pushing it

Release = tag push. I once ran `git commit` without `git add`, pushed an empty branch,
then tagged `main` - the release `v1.0.8` was byte-for-byte `v1.0.7`.

Before every release tag:

```bash
git checkout main && git pull
git log --oneline -1      # is this my merge?
git tag v1.0.9 && git push origin v1.0.9
```

And never move a pushed tag to fix it: just cut the next version.
