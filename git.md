# Git

## Pull Requests

PRs and code changes should be the smallest useful unit. This is because later the PR will be the best source of information for you and other developers. The more code that is in a particular PR, the more difficult it is to quickly understand what the code does, how it should be used, and why it was added.

The "size" of a PR can be measured in the number of lines of code added and removed, the number of files with changes, but also the number of different "types" of changes.

For example, a typical PR might have two types of changes: an added feature and an added test. Often, it is tempting to to "throw in" additional types of changes like refactoring or code style changes. However, this adds substantial noise to the PR. This additional noise makes it more difficult to understand the code at review time, when a change is needed, and if it needs debugging.

PRs consisting of only refactor or only code style changes are very quick to review because there is a low chance of introducing bugs.
