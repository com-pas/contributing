# Contributing to CoMPAS

First off, thanks for taking the time to contribute!

The following is a set of guidelines for contributing to the CoMPAS project. These are mostly guidelines, sometimes rules. Use your best judgment, and feel free to propose changes to this document in a pull request.

#### Table Of Contents

[Code of Conduct](#code-of-conduct)

[License and Developer Certificate of Origin](#license-and-developer-certificate-of-origin)

[How Can I Contribute?](#how-can-i-contribute)
  * [Reporting Bugs and Suggesting Enhancements](#reporting-bugs-and-suggesting-enhancements)
  * [Contributing Code](#contributing-code)
  * [Tools to contribute](#tools-to-contribute)
  * [Definition of Done](#definition-of-done)
  * [Copyright Guidelines](#definition-of-done)
  * [How-to begin](#how-to-begin)
  * [Github Project Boards](#github-project-boards)

[Styleguides](#styleguides)
  * [Copyright Guidelines](#copyright-guidelines)

[Project Governance](#project-governance)
  * [Project Owner](#project-owner)
  * [Technical Charter](#technical-charter)
  * [Committers](#committers)
  * [Technical Steering Committee](#technical-steering-committee)
  * [Contributors](#contributors)

## Code of Conduct

This project applies the [LF Energy Code of Conduct](https://www.lfenergy.org/about/code-of-conduct/). By participating, you are expected to uphold this code. Please report unacceptable behavior to the project's Technical Steering Committee [CoMPAS-tsc@lists.lfenergy.org](mailto:CoMPAS-tsc@lists.lfenergy.org).

## License and Developer Certificate of Origin

By contributing to the CoMPAS project, you accept and agree to the following terms and conditions for your present and future contributions submitted to CoMPAS.

All contributions to this project are licensed under the license stipulated at the corresponding sub-repository. Except where otherwise explicitely indicated, CoMPAS is an open source project licensed under the [Apache License, version 2.0](http://www.apache.org/licenses/LICENSE-2.0/). 

The project requires the use of the [Developer Certificate of Origin (DCO)](https://developercertificate.org/). The DCO is a legally binding statement asserting that you are you have the right to submit your contribution and to license it under the project's applicable license.

Contributors sign-off that they adhere to the term of the DCO by adding a ``Signed-off-by`` line to commit messages. The DCO sign-off must be attached to every contribution made by every contributor.

Here is an example ``Signed-off-by`` line, that indicates the contributor accepts the DCO:

````
This is my commit message.

Signed-off-by: Name Surname <name.surname@entity.com>
````
You can write it manually but Git even has a -s command line option to append this automatically to your commit message:
````
$ git commit -s -m 'This is my commit message'
````

Note that checks will be performed during the integration in order to require that commits in a Pull Request contain valid ``Signed-off-by`` lines.

## How Can I Contribute?

### Reporting Bugs and Suggesting Enhancements

Bugs and enhancement suggestions are tracked as [GitHub issues](https://guides.github.com/features/issues/). Create an issue and provide the following information by filling in [the template](ISSUE_TEMPLATE.md).

Before creating bug reports or suggesting enhancement, please **perform a [cursory search](https://github.com/search?q=+is%3Aissue+user%3Acom-pas)** to see if the problem has already been reported. If it has **and the issue is still open**, add a comment to the existing issue instead of opening a new one..

You can also contact the team directly to talk about your ideas at [CoMPAS-dev@lists.lfenergy.org](mailto:CoMPAS-dev@lists.lfenergy.org).

> **Note:** If you find a **Closed** issue that seems like it is the same thing that you're experiencing, open a new issue and include a link to the original issue in the body of your new one.

### Contributing Code

Code Contribution is tracked as [GitHub Pull Requests](https://help.github.com/en/articles/about-pull-requests). Crafting a good pull request takes time and energy and we will help as much as we can, but be prepared to follow our iterative process. The iterative process has several goals:

- maintain the software quality,
- fix problems that are important to users,
- engage the community in working toward the best possible software features,
- enable a sustainable system for maintainers to review contributions.

Please follow these steps to have your contribution considered by the maintainers:

1. Follow all instructions in [the template](PULL_REQUEST_TEMPLATE.md)
2. Follow the [styleguides](#styleguides)
3. After you submit your pull request, verify that all [status checks](https://help.github.com/articles/about-status-checks/) are passing.
5. Request a GitHub review by one of the projects' Committers
6. Follow their instructions or discuss about the requested changes. Please don't take criticism personally, it is normal to iterate on this step several times.
7. Repeat step 6 until the pull request is merged!

Continuous integration is setup to run on all branches automatically and will often report problems, so don't worry about getting everything perfect on the first try (SonarCloud Analysis is a notorious problem source). Until you add a reviewer, you can trigger as many builds as you want by amending your commits. The status checks enforce the following:

- All tests in the test suite pass.
- Checkstyle and SonarCloud report no violations.
- The code coverage is high enough (currently about 80%).

### Tools to contribute

Continuous integration is setup automatically on all contributions. However, it's faster to iterate locally to fix problems than waiting for the status checks to finish. There are many tools that can be used to do the verifications that are enforced by all status checks. The most simple and universal tool is maven, but IDE integrations can be used to get more immediate feedback. Most of the team uses IntelliJ IDEA, but others IDEs can be used, for exemple the Eclipse IDE.

### Definition of Done

Before finishing a requirement, you need to check if everything is done for that particular requirement.
For that, we have a Definition of Done; the DoD decides when a requirement is really done.

Note: A Defintion of Done is not a static list. It can be modified any time, if people feel like corrections should be made.

Current Definition of Done:
- Assumptions of requirements are met.
- Required documentation is done.
- (Software) Requirement is accepted and got a thumbs up from the Maintainer via an accepted Pull Request.
- (Software) The build succeeds without failures.
- (Software) All tests in the test suite pass.
- (Software) Code style checks report no violations.
- (Software) Code coverage is high enough (a minimum of 80% is coveraged).
- (Software) If applicable, the added Unit Test is written, executed and passed.
- (Software) Security analysis (vulnerability detection) doesn't spot unaddressed issues.

### How-to begin

Before you start your coding journey within the CoMPAS project, there are some things we have to talk about.
Some things that will make your start a little bit easier! 
On the [developing](DEVELOPING.md) page information about tooling can be found.

#### Open Community Calls
It's good to know that every other monday, we are having a so called Open Community Call. Everyone participating in the CoMPAS project can join and talk about and ask question about the CoMPAS project. 

When the Open Community Calls are taking place, can be found at the [General CoMPAS mailing list calendar](https://lists.lfenergy.org/g/CoMPAS/calendar).

The agendas can be found at the [LF Energy wiki](https://wiki.lfenergy.org/display/HOME/CoMPAS+Community+Calls).

If you have something to add, please add it to the agenda and notify everyone on Slack!

#### Slack channel
One of the first important things, is to meet the community. Feel free to introduce yourself on our [Slack channel](https://app.slack.com/client/TLU68MTML/C01926K9D39)!

The Slack channel is the first communication platform within the CoMPAS project (besides email and the Github platform), so if you need help for example you can use Slack!

#### Documenting
A good (open source) project requires documentation.
We have two places for our documentation

##### LF Energy Wiki
LF Energy has it's own [CoMPAS specific Wiki](https://wiki.lfenergy.org/display/HOME/CoMPAS). This is the place for documenation about CoMPAS in general (like roadmap and the community call agendas).

##### CoMPAS Architecture Github Pages
There is also a [Github Pages](https://com-pas.github.io/compas-architecture/) website for CoMPAS architecture specific topics.

#### Copyright and Licensing
Copyright and license information is done on per-file basis. We use the specification of [REUSE](https://reuse.software/spec/) 
to ensure that copyright information of the project is clear and can be analuzed in an automated fashion.

Every source code repository within CoMPAS has a Github Action for checking against the REUSE specification.

For more information, check the [Copyright Guidelines](#copyright-guidelines) section.

#### Architecture and technologies
For all architecture and technology choices (for example frameworks, build tools, database choices, etcetera), 
please check the source code (duh!) and our [CoMPAS Architecture Github Pages](https://com-pas.github.io/compas-architecture/).

#### Adding custom badges to your README
Badges are great for quickly checking several status reports of a specific repository.
Sometimes a application doesn't serve badges ([LFX Security tool](https://security.lfx.linuxfoundation.org/) for example), and you need to do it yourself.
We use [shields.io](https://shields.io/) for this problem.

In case of the LFX Security Tool, we used the following:
- Go to [shields.io](https://shields.io/).
- Go to the 'Dynamic' section.
- Choose JSON as data type.
- Insert 'LFX Security Tool' as the label.
- Insert the API to use, in case of our LFX Security tool projects we use [this API](https://api.security.lfx.linuxfoundation.org/v1/project/e8b6fdf9-2686-44c5-bbaa-6965d04ad3e1/issues).
- Now you can query using JsonPath. To get all open high issues from the 'CoMPAS Core' project, use `issues[?(@['repository-name'] == 'compas-core')]['high-open-issues']`.
- Choose a color and a pre- or surfix text.

### Github Project Boards
For managing the CoMPAS issues created in all the separate repositories, we use the [Projects Board](https://github.com/orgs/com-pas/projects) of Github.
CoMPAS uses 2 different Project Boards: One for [Pull Requests](https://github.com/orgs/com-pas/projects/2) and one for the [Issues](https://github.com/orgs/com-pas/projects/1).

When creating Pull Requests or Issues, it will automatically create issues or Pull Requests on the Project Boards.
This is done by the Automate Projects Github Action (take a look at the [action from the Data Service](https://github.com/com-pas/compas-scl-data-service/blob/develop/.github/workflows/automate_projects.yml) for example).
Changing the status of Issues / Pull Requests is also handled automatically by the Github Action.

Issues and Pull Requests can be moved on both the Project Boards and on the boards of the specific repository itself. It synchronizes automatically.

## Styleguides

### Copyright Guidelines

Contributing to the CoMPAS project also requires to use correct copyright headers in source files.

For each CoMPAS repository, we created / will create a Github Action featuring [REUSE](https://reuse.software/). REUSE is a piece of software which checks for correct copyright information in files defined in a [specification](https://reuse.software/spec/). The specification is based on best practices and the use of [SPDX](https://spdx.dev/) identifiers.

Example Alliander copyright header for Java files:
```Java
// SPDX-FileCopyrightText: 2020 Alliander N.V.
//
// SPDX-License-Identifier: Apache-2.0
```

Every commit on a Pull Request is being scanned by REUSE. If it fails, the pull requests cannot be merged.

For more tips on using REUSE (for example with a small command line tool), check the [Tips: Copyright & Licensing](https://wiki.lfenergy.org/pages/viewpage.action?pageId=10996220) wiki page.

### English language convention

The convention for all the project's documents, including code documentation, website, is to write American English.
A list of spelling differences between British and American English is available 
[here](https://www.britishcouncilfoundation.id/en/english/articles/british-and-american-english) for example.

## Project Governance

#### Project Owner

CoMPAS is part of the [LF Energy Foundation](https://www.lfenergy.org/), a project of The Linux Foundation that supports open source innovation projects within the energy and electricity sectors.

#### Technical Charter

The Project's [Technical Charter](CoMPAS%20Technical%20Charter%207-6-2020.pdf) sets forth the responsibilities and procedures for technical contribution to, and oversight of, the COMPAS Project.

#### Committers

Committers are contributors who have made several valuable contributions to the project and are now relied upon to both write code directly to the repository and screen the contributions of others. In many cases they are programmers but it is also possible that they contribute in a different role. Typically, a committer will focus on a specific aspect of the project, and will bring a level of expertise and understanding that earns them the respect of the community and the project owner.

#### Technical Steering Committee

The Technical Steering Committee (TSC) is composed of voting members elected by the active Committers as described in the project’s Technical Charter. The TSC is responsible for the technical direction of the project.

#### Members
CoMPAS TSC voting members are:
- Norbert Armand (https://github.com/Norbert-armand)
- Frédéric Fousseret (https://github.com/FredFousPro)
- Mohamed Sylla (https://github.com/syllamoh)
- Stevan Vigouroux (https://github.com/SteVigGE)
- Sander Jansen (https://github.com/Sander3003)
- Rob Tjalma (https://github.com/Flurb)

#### Voting
While the Project aims to operate as a consensus-based community, if any TSC decision requires a vote to move the Project forward, the voting members of the TSC will vote on a one vote per voting member basis. The simple majority is needed to approve proposals.
The preferred way to vote is to create a poll [here](https://lists.lfenergy.org/g/CoMPAS-tsc/addpoll).

#### Responsibilities
The project is split into several repositories. There is at least one Committer in charge of each repository. By "in charge", we mean:
-	best effort to review the pull request,
-	best effort to resolve issues,
-	building and publishing the releases, including writing the release notes and informing the community,
-	in case of unability to perform the above tasks, the Committer in charge has to ask the TSC through the list [CoMPAS-tsc@lists.lfenergy.org](mailto:CoMPAS-tsc@lists.lfenergy.org) to find another Committer to review the pull request, resolve the issue or build and publish the release.  

Please refer to our [maintainers file](MAINTAINERS.md) for more details about our work division.

#### Contributors

Contributors include anyone in the technical community that contributes code, documentation, or other technical artifacts to the Project.

Anyone can become a contributor. There is no expectation of commitment to the project, no specific skill requirements and no selection process. To become a contributor, a community member simply has to perform one or more actions that are beneficial to the project.
