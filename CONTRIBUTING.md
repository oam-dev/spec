# Contributing

## Introduction

The developing of Open Application Model is driven by KubeVela, in most cases, you should use KubeVela repository for contribution. This repository mainly accepts issues and PRs for API specification release planning and maintenance purposes.

## Proposing Changes and LGTM Policy

To propose a change or ask a question, please open an issue in the issue queue before submitting a pull request (PR). All PRs must be reviewed and approved (LGTMed) by 2 maintainers before being merged. Maintainers are specified in the [OWNERS](OWNERS.md) file. Refer to to the contribution guidelines in this document for more details on the process for issues and PRs.

## Code of Conduct

This project has adopted the [Contributor Covenant code of conduct](code-of-conduct.md/).
Please maintain respectful communication in the issue queue, PR queue, and all other communication channels.

## Contribution guidelines

This repository accepts contributions via GitHub pull requests. The following section outlines the process for merging contributions to the spec.

### Issues

Issues are used as the primary method for tracking anything to do with the Open Application Model project.

#### Issue Lifecycle

All issue types follow the same general lifecycle. Differences are noted below.

1. Issue creation
2. Triage
    - The maintainer in charge of triaging will apply the proper labels for the issue. This
    includes labels for priority, type, and metadata.
    - (If needed) Clean up the title to succinctly and clearly state the issue. Also ensure
    that proposals are prefaced with "Proposal".
    - We attempt to do this process at least once per work day.
3. Discussion
    - "Spec Change" issues should be connected to the PR that resolves it.
    - Whoever is working on a "Spec Change" issue should either assign the issue to themselves or make a comment in the issue
    saying that they are taking it.
    - "Proposal" and "Discussion" issues should stay open until resolved.
4. Issue closure

### How to Contribute a Patch

We use Pull Requests (PRs) to track code changes. To submit a change to the project:

1. Fork the repo, modify the project to address the issue.
2. Pick an open issue from the [issue list](https://github.com/oam-dev/spec/issues) and claim it in the comments. After approval fix the issue and send us a pull request (PR).
3. Or you can create a new issue. A community member will get back to you and, if approved, you can fix the issue and send a pull request.
4. Please go through our issue list first (open as well as closed) and make sure the issue you are reporting does not replicate an issue already reported. If you have issues on multiple pages, report them separately. Do not combine them into a single issue.
5. Submit a pull request to the appropriate branch. 

The next section contains more information on the workflow followed for Pull Requests.

#### PR Lifecycle

1. PR creation
    - We more than welcome PRs that are currently in progress. They are a great way to keep track of
    important work that is in-flight, but useful for others to see. If a PR is a work in progress,
    it **should** be prefaced with "WIP: [title]". You should also add the `wip` **label** Once the PR is ready for review, remove "WIP" from the title and label.
    - It is preferred, but not required, to have a PR tied to a specific issue. There can be
    circumstances where if it is a quick fix then an issue might be overkill. The details provided
    in the PR description would suffice in this case.
2. Triage
    - The maintainer in charge of triaging will apply the proper labels for the issue. This should
    include at least a size label, a milestone, and `awaiting review` once all labels are applied.
    See the [Labels section](#labels) for full details on the definitions of labels.
3. Reviewing/Discussion
    - All reviews will be completed using the GitHub review tool.
    - A "Comment" review should be used when there are questions about the spec that should be
    answered, but that don't involve spec changes. This type of review does not count as approval.
    - A "Changes Requested" review indicates that changes to the spec need to be made before they will be
    merged.
    - Reviewers should update labels as needed (such as `needs rebase`).
    - When a review is approved, the reviewer should add `LGTM` as a comment. 
    - Final approval is required by a designated owner (see `.github/CODEOWNERS` file). Merging is blocked without this final approval. Approvers will factor reviews from all other reviewers into their approval process.
4. PR owner should try to be responsive to comments by answering questions or changing text. Once all comments have been addressed,
   the PR is ready to be merged.
5. Merge or close
    - A PR should stay open until a Final Approver (see above) has marked the PR approved. 
    - PRs can be closed by the author without merging
    - PRs may be closed by a Final Approver if the decision is made that the PR is not going to be merged 

### Avoid the following mistakes!

-  Fix a new issue and submit a PR without reporting and getting it approved at first.
-  Fix an issue assigned to someone else and submit a PR before the assignee does.
-  Report issues which are previously reported by others. (Please check the both open and closed issues before you report an issue).
-  Suggest completely new features in the issue list. (Please use the mailing list/gitter channel for these kinds of suggestions. Use issue list to suggest bugs/features in the already implemented sections.)

## Git Setup

### Configure remotes

When a repository is cloned, it has a default remote called `origin` that points to your fork on GitHub, not the original repository it was forked from. To keep track of the original repository, you should add another remote named `upstream`:<br />
1. Get the path where you have your git repository on your machine. Go to that path in Terminal using cd. Alternatively, Right click on project in Github Desktop and hit ‘Open in Terminal’.<br />
2. Run `git remote -v`  to check the status you should see something like the following:<br />
> origin    https://github.com/YOUR_USERNAME/spec.git (fetch)<br />
> origin    https://github.com/YOUR_USERNAME/spec.git (push)<br />
3. Set the `upstream`:<br />
 `git remote add upstream https://github.com/oam-dev/spec.git`<br />
4. Run `git remote -v`  again to check the status, you should see something like the following:<br />
> origin    https://github.com/YOUR_USERNAME/spec.git (fetch)<br />
> origin    https://github.com/YOUR_USERNAME/spec.git (push)<br />
> upstream https://github.com/oam-dev/spec.git  (fetch)<br />
> upstream  https://github.com/oam-dev/spec.git (push)<br />
5. To update your local copy with remote changes, run the following:<br />
`git fetch upstream master`<br />
 `git rebase  upstream/master`<br />
This will give you an exact copy of the current remote, make sure you don't have any local changes.<br />
6. Project set-up is complete.


### Contributing and developing a feature

1. Make sure you are in the develop branch `git checkout master`.<br />
2. Sync your copy with the upstream.<br />
3. Create a new branch with a meaningful name `git checkout -b branch_name`.<br />
4. Add the files you changed `git add file_name` (avoid using `git add .`).<br />
5. Commit your changes `git commit -m "Message briefly explaining the feature"`.<br />
6. Keep one commit per feature. If you forgot to add changes, you can edit the previous commit `git commit --amend`.<br />
7. Push to your repo `git push origin branch-name`.<br />
9. Go into [the Github repo](https://github.com/oam-dev/spec.git) and create a pull request explaining your changes.<br />
10. If you are requested to make changes, edit your commit using `git commit --amend`, push again and the pull request will edit automatically.<br />
11. If you have more than one commit try squashing them into single commit by following command:<br />
 `git rebase -i origin/master~n master`(having n number of commits).<br />
12. Once you've run a git rebase -i command, text editor will open with a file that lists all the commits in current branch, and in front of each commit is the word "pick". For every line except the first, replace the word "pick" with the word "squash".<br />
13. Save and close the file, and a moment later a new file should pop up in  editor, combining all the commit messages of all the commits. Reword this commit message into meaningful one briefly explaining all the features, and then save and close that file as well. This commit message will be the commit message for the one, big commit that you are squashing all of your larger commits into. Once you've saved and closed that file, your commits of current branch have been squashed together.<br />
14. Force push to update your pull request with command `git push origin branchname --force`.<br/>
