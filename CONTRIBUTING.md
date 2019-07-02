# Contributing Guidelines

The Hydra Specification project accepts contributions via GitHub pull requests. This document outlines the process for merging contributions to the spec.

## Issues

Issues are used as the primary method for tracking anything to do with the Hydra Specification project.

### Issue Types

There are 4 types of issues (each with their own corresponding [label](#labels)):
- Discussion: These are support or functionality inquiries that we want to have a record of for
future reference. Depending on the discussion, these can turn into "Spec Change" issues.
- Proposal: Used for items that propose a new ideas or functionality that require
a larger discussion. This allows for feedback from others teams before a
spec change is actually written. All issues that are proposals should
both have a label and an issue title of "Proposal: [the rest of the title]." A proposal can become
a "Spec Change" and does not require a milestone.
- Spec Change: These track specific spec changes and ideas until they are complete. They can evolve
from "Proposal" and "Discussion" items, or can be submitted individually depending on the size.

### Issue Lifecycle

The issue lifecycle is mainly driven by the core maintainers, but is good information for those
contributing to Hydra Specification. All issue types follow the same general lifecycle. Differences are noted below.
1. Issue creation
2. Triage
    - The maintainer in charge of triaging will apply the proper labels for the issue. This
    includes labels for priority, type, and metadata.
    - (If needed) Clean up the title to succinctly and clearly state the issue. Also ensure
    that proposals are prefaced with "Proposal".
    - We attempt to do this process at least once per work day.
3. Discussion
    - "Spec Change" issues should be connected to the PR that resolves it.
    - Whoever is working on a "Spec Change" issue should either assign the issue to themself or make a comment in the issue
    saying that they are taking it.
    - "Proposal" and "Discussion" issues should stay open until resolved.
4. Issue closure

## How to Contribute a Patch

1. Fork the repo, modify the specification to address the issue.
1. Submit a pull request.

The next section contains more information on the workflow followed for Pull Requests.

## Pull Requests and Issues

Like any good open source project, we use Pull Requests (PRs) to track code changes.

### PR Lifecycle

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
3. Assigning reviews
    - All PRs require 1 review approval from at least two teams before it can be merged. 
    - The teams currently are: 
      - Service Fabric Mesh (SFM)
      - Office of the CTO (OCTO)
      - Steelthread (ST)
4. Reviewing/Discussion
    - All reviews will be completed using Github review tool.
    - A "Comment" review should be used when there are questions about the spec that should be
    answered, but that don't involve spec changes. This type of review does not count as approval.
    - A "Changes Requested" review indicates that changes to the spec need to be made before they will be
    merged.
    - Reviewers should update labels as needed (such as `needs rebase`).
    - When a review is approved, the reviewer should add `LGTM` as a comment and the `LGTM <team-name>` label. 
    - Final approval is required by a designated owner (see `.github/CODEOWNERS` file). Merging is blocked without this final approval. Approvers will factor reviews from all other reviewers into their approval process.
5. PR owner should try to be responsive to comments by answering questions or changing text. Once all comments have been addressed,
   the PR is ready to be merged.
6. Merge or close
    - A PR should stay open until a Final Approver (see above) has marked the PR approved. 
    - PRs can be closed by the author without merging
    - PRs may be closed by a Final Approver if the decision is made that the PR is not going to be merged 

## The Triager

Each day, someone from a Hydra related team should act as the triager. This person will be in charge triaging new PRs and issues throughout the day. Anyone can volunteer as the traiger. If no one has volunteered by 10:00 AM PST, someone from Steelthread will triage. 

## Labels

The following tables define all label types used for Hydra Specification. It is split up by category.

### Common

| Label | Description |
| ----- | ----------- |
| `high priority` | Marks an issue or PR as critical. This means that addressing the PR or issue is top priority and will be handled first |
| `duplicate` | Indicates that the issue or PR is a duplicate of another |
| `spec change` | Marks the issue or a PR as an agreed upon change to the spec |
| `wip` | Identifies an issue or PR as a work in progress | 

### Issue Specific

| Label | Description |
| ----- | ----------- |
| `proposal` | This issue is a proposal |
| `discussion` | This issue is a question or discussion point to capture feedback |

### PR Specific

| Label | Description |
| ----- | ----------- |
| `awaiting review` | The PR has been triaged and is ready for someone to review |
| `needs rebase` | A helper label used to indicate that the PR needs to be rebased before it can be merged. Used for easy filtering |
| `lgtm <teamname>` | A label to indicate that a team (sfmesh, steelthread, octo) approves. |

#### Size labels

Size labels are used to indicate how much change is in a given PR. This is helpful for estimating review time. 

| Label | Description |
| ----- | ----------- |
| `size/small` | Anything less than or equal to 4 files and 150 lines. |
| `size/medium` | Anything greater than `size/small` and less than or equal to 8 files and 300 lines. |
| `size/large` | Anything greater than `size/medium`. This also should be applied to anything that is a significant specification change. |

### Milestones

The Hydra Specification will work toward a set of milestones. PRs and issues can have milestones associated with them. The triager should assign milestones as appropriate. The milestones will be created per working group agreement. 
