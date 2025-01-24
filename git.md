# La Suite numérique with Git

[Git](https://git-scm.com) is used as a [DVCS](https://en.wikipedia.org/wiki/Distributed_version_control) for every La Suite numérique project involving code or documentation. In this chapter, you will find how we use Git on a daily-basis to code and collaborate with the team.

If you are looking for a good introduction to Git, take a look at the [_Git In Practice_](https://github.com/GitInPractice/GitInPractice#readme) book from Mike McQuaid.

## Git conventions

### Commit granularity

We tend to favor low granularity-but-consistent commits to a long series of commit. If you have multiple commits in a feature branch, it means that you had to address multiple issues to achieve your feature. We can run the test suite on every commit and it should always stay green 😎.

### Commit message

We follow an emoji-driven commit message format adapted from [Angular guidelines](https://github.com/angular/angular/blob/master/CONTRIBUTING.md#-commit-message-guidelines). A typical commit message should look like:

```
<type>(<scope>) <subject>
<BLANK LINE>
<body>
<BLANK LINE>
<footer>
```

### Type, scope and subject

Where **type** must be an emoji unicode character (e.g. 🎉 **and not** `:tada:`) chosen from the [gitmoji guide](https://gitmoji.carloscuesta.me/).

And the **scope** should point to the django application or stack component that may be affected, _e.g._:

* docker
* apps:core
* plugins:foo

The **subject** contains succinct description of the change:

* use the imperative, present tense: "change" not "changed" nor "changes"
* don't capitalize first letter
* no dot \(.\) at the end


💩 Bad

```
:sparkles:(auth) add support for HTTP basic auth
✨(Auth.py) add support for HTTP basic auth
✨(auth) add support for HTTP basic auth.
✨(auth) Add support for HTTP basic auth
✨(auth)add support for HTTP basic auth
✨(auth) added support for HTTP basic auth
```

❤️ Good

```
✨(auth) add support for HTTP basic auth
```


#### Body

Just as in the subject, use the imperative, present tense for the **body**: "change" not "changed" nor "changes". The body should include the motivation for the change and contrast this with the previous behavior.

In other words, you should explain WHY you do what you do, and what was the context
that led you to do it the way you do it. It should not be a list of the things
you change, because that's what `git diff` is here for.

Try to see the commit message as a "brain dump" of the state of your mind at the
moment of committing the changes.

You can also imagine someone angry chasing you in 6 months and asking:

> Why on earth did you do this ??

You surely wouldn't answer with a list of what you did. You would try to explain and
justify what you did 6 months ago. The problem is that, at that
time, you will have forgotten about all these details... and your changes may
be misunderstood. This may lead to tensions and criticism on the quality of your work.
Worse, someone may revert an important change for lack of understanding the implications.
Ironically, the angry person will most probably be... the future you!

Here is an example of a good versus a bad commit body:

💩 Bad

```
Deactivate the French language in production and activate it in development.
```

❤️ Good

```
Translators are still working on the French language, which will not
be ready before we go live on 2020/09/01. We must hide it in production
so that users don't see it while it is incomplete. However, we need to
keep it activated in the development environment so that the language
selector is displayed to the developper, and also because the demo site
that we use for develoment assumes that french and english are both
activated. The demo site generator would fail if we deactivated French
in development.
```

#### Footer

The **footer** should contain any information about breaking changes and is also the place to reference GitHub/GitLab issues that this commit closes.

Breaking changes should start with the word `BREAKING CHANGE`: with a space or two newlines. The rest of the commit message is then used for this.



### Git `commit-msg` hook

Commits are usually checked in the project CI, but you can install [gitlint](https://jorisroovers.com/gitlint/latest/commit_hooks/#commit-msg-hook) as a git `commit-msg` hook so that gitlint checks your commit messages locally after each commit.

For this, install the `gitlint` package globally using `pip`:
```
pip install gitlint
```

Install the `commit-msg` hook:
```
gitlint install-hook
```

Note that `gitlint` cannot work together with an existing hook. If you already have a `.git/hooks/commit-msg` file in your local repository, `gitlint` will refuse to install the `commit-msg` hook.

`gitlint` should now be runned locally for each commits you make.

If you want to manually trigger `gitlint` for your last commit message, use the following command:
```
gitlint --msg-filename .git/COMMIT_EDITMSG
```



## Git workflows

We use different workflows to handle collaborative coding with Git. The chosen workflow depends on the context and historical reasons. If you need more insights on popular Git worflows, we invite you to read [this section](https://github.com/GitInPractice/GitInPractice/blob/master/14-RecommendedTeamWorkflows.adoc) of the [Git in Practice](https://github.com/GitInPractice/GitInPractice) book.

### Open-source projects

For the sake of simplicity, to ease interaction with the community, we use the [GitHub flow](https://guides.github.com/introduction/flow/index.html) for open-source projects. In a few words:

* the `master` branch is always stable and deployable,
* tags from the master branch are considered as releases,
* contributors have to fork or create a new feature-branch to work on \(if they are allowed to in the original repository\) and propose a pull request to merge their branch to `master`.

### La Suite numérique private projects

Historically, we use [Git Flow](http://nvie.com/posts/a-successful-git-branching-model/) for internal projects. You will find plenty of resources on the web about this workflow, so in a few words:

* the `master` branch is considered as a stable and always deployable branch,
* the `develop` branch is a working branch where developped features are merged,
* contributors have to work on feature-branch that will target the `develop` branch,
* hotfix and release branches are merged to `master` and back-ported to `develop`,
* release branches create tags on the master branch when they are closed.

## Working with forges

### Declaring issues

When declaring a new issue, please describe as much as possible the **purpose** of your issue, and eventually make a **proposal** on how it should be solved or investigated. Choose wisely a label for this issue and please do not assign someone to it \(unless you already discussed with her verbally and had an agreement\).

### Working with pull requests \(PR\)

We recommend to create a pull request \(PR\) or merge request \(MR\) in GitLab semantic as soon as possible with the `WIP` flag preceding your PR title, _e.g._ `WIP: 😎(docker) add mongo service`.

When your work is done on this PR, remove the `WIP` flag and please ensure that:

* your feature or fix is **tested ** \(all continuous integration tests should be green and code coverage **should not** decrease\),
* your feature is **documented**,
* your branch is **up-to-date** with the target branch \(it should be rebased and force-pushed\).

Once all of those requirements are met, ask for a review of your code by assigning maintainers to your PR. Your changes should be approved by **at least one contributor of the core team** to be merged.

Last but not least, a code review should not take more than half an hour per PR to be profitable for everyone. It means that you have anticipated the amount of changes required to achieve your work. If those changes are bigger than 500 lines, then you may consider to split your feature in multiple PRs.

Note that the target branch \(`develop` or `master`\) will be write-protected, _i.e._ no one is allowed to push to it. Hence you will need to use the forge UI to merge your PR once all tests are green and your changes have been approved. Our PR merging strategy is: **rebase and merge** ; we do not want a merge commit.

## Releasing new software version

Whatever the language you are using on a La Suite numérique project, cooking a new release \(_e.g._ `4.18.1`\) should follow a standard procedure described below:

1. Create a new branch named: `release/4.18.1`,

2. Bump the release number in the appropriate file, _i.e._ for python projects, the `setup.cfg`  \(or `__init__.py`, depending on the way you handle your package version\) and/or `package.json` for node-based projects,

3. Update the project's `Changelog` following the [keepachangelog](https://keepachangelog.com/en/0.3.0/) recommandations,

4. Commit your changes with a structured message:

    - add a title including the version of the release and respecting the above described format using the 🔖 release emoji,
    - paste in the body all changes from the changelog concerned by this release, removing only the markdown tags and making sure that lines are shorter than 74 characters.

        ```
        🔖(minor) bump release to 4.18.0

        Added:

        - Implement base CLI commands (list, extract, fetch & push) for supported
        backends
        - Support for ElasticSearch database backend
        
        Changed:

        - Replace LDP storage backend by FS storage backend
        ```

5. Open a pull or merge request depending on the current forge of the project,

6. Wait for an approval from your peers,

7. Merge your pull or merge request,

8. Checkout and pull changes from the `master` branch,

9. Tag & push your commit: `git tag v4.18.1 && git push origin --tags`

10. Unless the CI has already taken care of it (check on the Circle CI config if there is a release job defined), manually release your version on GitHub.

11. Ensure your release is published on the package index.

### Checking project tags consistency

As we are only Humans, we are error-prone _per se_. To avoid tagging consistency errors, we recommend to integrate the following tests in the project's continuous integration workflow before publishing a new release:

```bash
# Get current tag corresponding commit ID
tag_commit=$(git rev-list -n 1 $CIRCLE_TAG)

# Check that the tag refer to a commit in the $TARGET_BRANCH (e.g.
# the master branch)
git branch -a --contains ${tag_commit} | grep ${TARGET_BRANCH}

# Check that the current tag (vX.Y.Z) matches the release number in
# setup.cfg (X.Y.Z)
grep "$(echo $CIRCLE_TAG | sed 's/^v/version = /')" setup.cfg
```

In this example script `$CIRCLE_TAG` is an environment variable defined by the contious integration platform \(CircleCI in this case\) with the pushed tag value, and, `$TARGET BRANCH` is the `Git` branch that should have been tagged \(e.g. the `master` branch\).

