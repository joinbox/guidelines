# Git Branching Model

Joinbox uses a more specific version of the git-flow branching model

![Joinbox Git Flow Model](git-flow.png "Joinbox Git Flow")


## General

- the master branch can be released at any time, it is always 100% stable
- the develop branch contains only code that works and is stable but not yet tested by the client
- features and bug fixes are developed in feature branches until they are stable. the base branch for creating feature branches is the release branch where the issue is in or the master branch for hot fixes.
- feature branches may only be merged into the release branch if they are tested and work correctly.
- feature branches will only be merged in to the according branch after at least one reviewer approved the pull request.
- for each iteration (release cycle, milestone) a new release branch is created from develop branch when the development starts.
- the release branch is published to a testing server for acceptance tests by the client. the client should receive the change log of the release branch
- fixes for the release branch are merged into the release branch
- the release branch may be merged into develop or another release branch if it is required
- hot fixes are only created if they must be applied to the master branch for immediate release. they are based on the master branch
- after a hot fix or release was released the master branch is merged into the develop branch as well as open release branches
- releases must be planned in advance, there should not be more than one release branch at any time
- after the release branch was merged into master and tagged with the correct version the release branch may be deleted.

## Naming
Branches must not be updated after a review was requested, before changes are requested in the review. If the developer want's to track the progress, a pull request without a reviewer assigned may be opened.

- A feature branch must be created for each issue based on the release branch. The name must follow the pattern `feature/iss-<Issue-Number>-<Short-Description-of-the-Issue>`. There may be multiple feature branches for one issue.
- A release branch must be created for each milestone based on the current develop branch. The name must follow the pattern `release/<Milestone-Name>`
A fix branch may be created for bugs or forgotten parts of a planed issue ( inside a milestone ) based on the release branch. The name must follow the pattern `fix/iss-<Issue-Number>-<Short-Description-of-the-Issue>`
- A hotfix branch may be created based on the current master branch, to fix a critical bug on the live system. Hofixes are not part of a planned release and will be released ASAP. The name must follow the pattern `hotfix/iss-<Issue-Number>-<Short-Description>`. Hotfixes may not be tested by the customer on the test system.
