# Commit Messages

The commit messages must follow the rules in this document.

As we build automated change logs for some projects, you have to use the specified types and formats described hereafter.
The scope may be optionally set.
Note: the software creating the versions may not bump the version if there is neither a feature nor a fix since the last release. Please read the documentation form the used software for further information.


The types are based on [Sparkbox's article](https://seesparkbox.com/foundry/semantic_commit_messages) about semantic commit messages.
There is a [cli](https://github.com/fteem/git-semantic-commits) to use the commit messages with a simpler interface

## Commit Message Format

```
feat(scope): add hat wobble
^--^  ^------------^
|     |
|     +-> Summary in present tense.
|
+-------> Type: chore, docs, feat, fix, refactor, style, or test.

```
- feat: mandatory, one of the listed types from below.
- summary: mandatory, summary about your changes in present tense.
- scope: optional, describes the component the code is changing.

## Type

Must be one of the following:

- chore: A task or a small change not affecting the functionality of the code
. docs: Changes or additions in the documentation.
- feat: A feature.
- fix: A bug fix.
- refactor: A code change that neither fixes a bug nor adds a feature
- style: Changes that do not affect the meaning of the code (white-space, formatting, missing semi-colons, etc)
- test: Adding missing tests or correcting existing test.
