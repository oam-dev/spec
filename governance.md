# Governance

This document defines project governance for the project.


## Steering Committee

Steering committee members are responsible for the long term success and adoption of the Open Application Model (OAM) through technical vision and strong commitment to both the projects and community.

Responsibilities include:

* Defining and maintaining the technical vision and overall direction of the OAM project.
* Taking [maintainer responsibilities](#Maintainer-Role) for the [OAM specification](https://github.com/oam-dev/spec).
* Approving new projects, or archiving projects in `oam-dev` organization.
* Accepting feedback from community and mapping to OAM specification.
* Participating in the conflict resolution and employing the "[organization voting](#Voting)" process when necessary.
* Actively participate in the regularly scheduled community meetings.
* Defining common practices and guidelines to OAM implementations in the community.

The current list of steering committee members is published and updated in [OWNERS.md](OWNERS.md#steering-committee).

### Becoming a Steering Committee Member

Steering committee is **organization** based.
- Each organization can have multiple members in the steering committee but only **one** vote.
- If a steering committee member changes company, his/her membership can not carry on and must be replaced by someone else.
- All steering committee members must meet following requirements.

Requirements:

- Showed up for 3 of the last 5 OAM community meetings.
- Made significant contributions to OAM specification.
- Or, maintainers of existing OAM implementations in the open source community.
    - implementation will be reviewed by existing steering committee members.
- Or, key members behind OAM based products or cloud offerings.
- Or, anyone has demonstrated consistent vision and input for the good of the OAM community with the big picture in mind.
    - Final qualification decisions will be made by existing steering committee members.

If you meet above requirements and are interested in joining the steering committee, please update [OWNERS.md](OWNERS.md#steering-committee) by a pull request as self-nomination. Existing steering committee members can also nominate anyone following the same process. Once qualified, steering committee will call for a [vote](#Voting).

### Removing a steering committee member

If a steering committee member is no longer interested or cannot perform the duties listed above, they should volunteer to be moved to emeritus status. Once volunteered, steering committee will call for a [vote](#Voting). In extreme cases, removing existing member can also occur by a vote of the steering committee.

### Nonaffiliated individual

Individuals not associated with or employed by a company or organization are allowed as members of steering committee while this is considered as **rare case**. Clarification will be required in the nomination PR to explain its necessity and must be verified and approved by steering committee.

## Maintainer Role

Maintainer role is per project. Maintainers have the most experience with the project under `oam-dev` organization and are expected to lead its growth and improvement.

Responsibilities include:

* Strong commitment to the project.
* Participate in design and technical discussions.
* Contribute non-trivial pull requests.
* Perform code reviews on other's pull requests.
* Regularly triage GitHub issues.
* Make sure that ongoing PRs are moving forward at the right pace or closing them.
* Monitor OAM mailing list, slack channel and response to social media interactions.
* Regularly attend the community meetings.

The current list of maintainers is published and updated in `OWNERS.md` of every project.

### Becoming a maintainer

To become a maintainer of [OAM specification](https://github.com/oam-dev/spec): 
- OAM specification is maintained by the OAM steering committee members **only**. Please check the [requirements of becoming steering committee member](#Becoming-a-Steering-Committee-Member).


To become a maintainer of other projects under `oam-dev` organization:

- Active contributions for at least 1 month.
- Get at least 10 non-trivial pull requests merged to the codebase.
- Primary reviewer for at least 3 non-trivial pull requests from others.
- Done through PR to update the project’s OWNERS file.
- Self-nominate or be nominated by existing maintainers in the project.
- With no objections from existing maintainers in the project.

### Removing a maintainer

If a maintainer is no longer interested or cannot perform the maintainer duties listed above, they should volunteer to be moved to emeritus status. Once volunteered, it will automatically take effect unless objection is raised from existing maintainers. In extreme cases removing maintainers can also occur by a vote of steering committee.


### Github Project Administration

Maintainers will be added to the OAM GitHub organization (if they are not already) and added to the GitHub Maintainers team.

## Voting

In general, we prefer that technical issues be resolved at project level by project maintainers. But if a dispute cannot be decided, the steering committee can be called in to decide an issue. If the steering committee members themselves cannot decide an issue, the issue will be resolved by voting. 

- All changes in governance, additions and removals of steering committee members require a **2/3 majority**, while other decisions and changes require only a simple majority.
- Only steering committee members can vote.
- Organization voting:
    - Each organization (regardless of the number of steering committee members associated with or employed by that company/organization) receives **one** organization vote.
    - [Nonaffiliated Individual](#Nonaffiliated-individual) receives one organization vote. 

For formal votes, a specific statement of what is being voted on should be added to the relevant github issue or PR, and a link to that issue or PR added to the maintainers meeting agenda document. Voters should indicate their yes/no vote on that issue or PR, and after a suitable period of time, the votes will be tallied and the outcome noted.

## Code of Conduct
This project has adopted the [Contributor Covenant code of conduct](code-of-conduct.md/).
