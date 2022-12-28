# Release Workflow

## Preparations

### Lint

Make sure to have one last Pull request to remove as much lint as possible. Same goes for deprecation warnings, linter exceptions etc.

### Authors

Update the [.mailmap](.mailmap) and [AUTHORS](AUTHORS) files:

```
git log --use-mailmap | grep '^Author:' | cut -f2- -d' ' | sort | uniq > AUTHORS
```
