<!--
# SPDX-License-Identifier: CC0-1.0
# SPDX-FileCopyrightText: Authors
-->

# Contributing 

Thank you for contributing!

Our aim is to be helpful to the community.
We therefore appreciate your feedback, input, and suggestions to this project.
We welcome issues and pull requests from everyone.

Please note that while we intend to respond to each contribution if and when possible, we may not always have the capacity to do so.

This is because the working group decides what gets included in the development version of the Minimum Interoperability Mechanisms. 
[Read our GOVERNANCE.md](/GOVERNANCE.md) to find out more.

> If you're not comfortable with GitHub, please [email us](mailto:claus@oascities.org) your feedback.

## Problems, suggestions and questions in issues

Please help development by reporting problems, suggesting changes and asking questions.

To do this, you can [create a GitHub issue](https://docs.github.com/en/issues/tracking-your-work-with-issues/creating-an-issue).

You don't need to change any of our documentation to be a contributor!

Questions and feedback are also very helpful.

## Documentation in pull requests

If you want to add to the documentation you can make a pull request.

If you never used GitHub, get up to speed with [Understanding the GitHub flow](https://docs.github.com/en/get-started/quickstart/github-flow) or follow one of the great free interactive courses in [GitHub Skills](https://skills.github.com/) on working with GitHub and working with MarkDown, the syntax this project's documentation is in.

This project is licensed Creative Commons Zero v1.0 Universal, which essentially means that the project, along with your contributions is in the public domain in whatever jurisdiction possible, and everyone can do whatever they want with it.

### 1. Make your changes

This project uses the [GitFlow branching model and workflow](https://nvie.com/posts/a-successful-git-branching-model/).
When you've forked this repository, please make sure to create a feature branch following the GitFlow model.

Add your changes in commits [with a message that explains them](https://thoughtbot.com/blog/5-useful-tips-for-a-better-commit-message).
If more than one type of change is needed, group logically related changes into separate commits.
For example, white-space fixes could be a separate commit from text content changes.
When adding new files, select file formats that are easily viewed in a `diff`, for instance, `.svg` is preferable to a binary image.
Document choices or decisions you make in the commit message, this will enable everyone to be informed of your choices in the future.

If you are adding code, make sure you've added and updated the relevant documentation and tests before you submit your pull request.
If possible, write tests that show the behavior of the newly added or changed code.

#### Style

The Minimum Interoperability Mechanisms aim to [use plain English](criteria/use-plain-english.md) and we have chosen British English for spelling.
Text content should typically follow one line per sentence, with no line-wrap, in order to make `diff` output easier to view.
However, we want to emphasize that it is more important that you make your contribution than worry about spelling and typography.
We will help you get it right in our review process.

#### Standards to follow

These are the standards that the Minimum Interoperability Mechanisms uses.
Please make sure that your contributions are aligned with them so that they can be merged more easily.

* [IETF RFC 2119](https://tools.ietf.org/html/rfc2119) - for requirement level keywords
* [Web Content Accessibility Guidelines 2.1](https://www.w3.org/WAI/WCAG22/quickref/?showtechniques=315#reading-level) - for readability

### 2. Pull request

When submitting the pull request, please accompany it with a description of the problem you are trying to solve and the issue number that this pull request fixes.
It is preferred for each pull request to address a single issue where possible.
In some cases a single set of changes may address multiple issues, in which case be sure to list all issue numbers fixed.

### 3. Improve

All contributions have to be reviewed by someone.

It could be that your contribution can be merged immediately by a maintainer.
However, usually, a new pull request needs some improvements before it can be merged.
Other contributors (or helper robots) might have feedback.
If this is the case the reviewing maintainer will help you improve your documentation and code.

If your documentation and code have passed human review, it is merged.
