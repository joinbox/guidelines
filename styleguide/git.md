# Git Branching Model

Joinbox uses a more specific version of the git-flow branching model

![Alt text](https://github.com/joinbox/guidelines/raw/master/styleguide/git-flow.png "Joinbox GIT Flow")


- the master branch can be released at any time, it is always 100% stable
- the develop branch contains only code that works and is stable but not yet tested by the client
- features and bug fixes are developed in feature branches until they are stable. the base branch for creating feature branches is develop.
- feature branches may only merged into the develop branch if they are tested and work correctly
- for each iteration (release cycle, milestone) a new release branch is created from develop as soon everything is ready.
- the release branch is published to a testing server for acceptance tests by the client. the client should receive the change log of the release branch
- fixes for the release branch are merged into the release branch. Branches for a release fix are based on the HEAD of the affected release branch. They may be merge into other release branches if needed.
- before the live release, the release branch must be merged into the develop- and master-branch. The master branch muts be tagged and the changelog needs to be generated.
- before a new release branch is created the old release branch(es) (if they exist) needs to be merged back into develop
- hot-fixes branches are only created for an immediate release. they are based on the master branch. they must be merged into to the master- and develop-branch 
- releases must be planned in advance, there should not be more than one release branch at any time
- after the release branch was merged into master- and develop-branch and tagged with the correct version the release branch may be deleted.

