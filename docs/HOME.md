# Contributing to CoMPAS

First off, thanks for taking the time to contribute!

The following is a set of guidelines for contributing to the CoMPAS project. These are mostly guidelines, sometimes rules. 
Use your best judgment, and feel free to propose changes to this document in a pull request.

#### Table Of Contents

[Code of Conduct](#code-of-conduct)

[License and Developer Certificate of Origin](#license-and-developer-certificate-of-origin)

[How Can I Contribute?](#how-can-i-contribute)
* [Community refinements](#community-refinements)
* [Reporting Bugs and Suggesting Enhancements](#reporting-bugs-and-suggesting-enhancements)
* [Contributing Code](#contributing-code)
* [Tools to contribute](#tools-to-contribute)
* [Definition of Done](#definition-of-done)
* [Copyright Guidelines](#definition-of-done)
* [How-to begin](#how-to-begin)
* [Copyright and Licensing](#copyright-and-licensing)
* [Github Project Boards](#github-project-boards)

[Github Pages](#github-pages)


## Code of Conduct

This project applies the [LF Energy Code of Conduct](https://www.lfenergy.org/about/code-of-conduct/). 
By participating, you are expected to uphold this code. Please report unacceptable behavior to the project's 
Technical Steering Committee [CoMPAS-tsc@lists.lfenergy.org](mailto:CoMPAS-tsc@lists.lfenergy.org).

## License and Developer Certificate of Origin

By contributing to the CoMPAS project, you accept and agree to the following terms and conditions for your present and future contributions submitted to CoMPAS.

All contributions to this project are licensed under the license stipulated at the corresponding sub-repository. 
Except where otherwise explicitely indicated, CoMPAS is an open source project licensed under the [Apache License, version 2.0](http://www.apache.org/licenses/LICENSE-2.0/).

The project requires the use of the [Developer Certificate of Origin (DCO)](https://developercertificate.org/). 
The DCO is a legally binding statement asserting that you are you have the right to submit your contribution and to license it under the project's applicable license.

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

## Community refinements

Every monday there is a CoMPAS Community Refinement at 10AM CET. The event is available at the CoMPAS mailing list [calendar](https://lists.lfenergy.org/g/CoMPAS/calendar) and can be subscribed to.

### Prepared topics

Every friday before the refinement meeting on Monday, all topics are published on the #compas channel on our [LFEnergy Slack](https://lfenergy.slack.com/).

### Add your own topics

Everybody can suggest topics for the refinement. To do this, join the [LFEnergy Slack](https://lfenergy.slack.com/) if not already, and put your topics in the #compas channel. You can just do this by writing a message like "I want to add something to the refinement this monday, namely..." or add a comment to the already prepared topics if available (see [Prepared topics](#prepared-topics)).

### Reporting Bugs and Suggesting Enhancements

Bugs and enhancement suggestions are tracked as [GitHub issues](https://guides.github.com/features/issues/). Create an issue 
and provide the following information by filling in [the template](ISSUE_TEMPLATE.md).

Before creating bug reports or suggesting enhancement, please **perform a [cursory search](https://github.com/search?q=+is%3Aissue+user%3Acom-pas)** 
to see if the problem has already been reported. If it has **and the issue is still open**, add a comment to the existing issue instead of opening a new one.

You can also contact the team directly to talk about your ideas at [CoMPAS-dev@lists.lfenergy.org](mailto:CoMPAS-dev@lists.lfenergy.org).

> **Note:** If you find a **Closed** issue that seems like it is the same thing that you're experiencing, open a new issue and include a link to the original issue in the body of your new one.

### Contributing Code

Code Contribution is tracked as [GitHub Pull Requests](https://help.github.com/en/articles/about-pull-requests). 
Crafting a good pull request takes time and energy, but we will help as much as we can, but be prepared to follow our iterative process. 
The iterative process has several goals:

- maintain the software quality,
- fix problems that are important to users,
- engage the community in working toward the best possible software features,
- enable a sustainable system for maintainers to review contributions.

Please follow these steps to have your contribution considered by the maintainers:

1. Follow all instructions in the [pull requests section](PULL_REQUESTS.md)
2. Follow the [styleguides](STYLEGUIDE.md)
3. After you submit your pull request, verify that all [status checks](https://help.github.com/articles/about-status-checks/) are passing.
5. Request a GitHub review by one of the projects' Committers
6. Follow their instructions or discuss the requested changes. Please don't take criticism personally, it is normal to iterate on this step several times.
7. Repeat step 6 until the pull request is merged!

Continuous integration is set up to run on all branches automatically and will often report problems, so don't worry about getting everything perfect on the first try 
(SonarCloud Analysis is a notorious problem source). Until you add a reviewer, you can trigger as many builds as you want by amending your commits. The status checks enforce the following:

- All tests in the test suite pass.
- Checkstyle and SonarCloud report no violations.
- The code coverage is high enough (currently about 80%).

### Tools to contribute

Continuous integration is setup automatically on all contributions. However, it's faster to iterate locally to fix problems than waiting for the status checks to finish. 
There are many tools that can be used to do the verifications that are enforced by all status checks. The most simple and universal tool is maven, but IDE integrations 
can be used to get more immediate feedback. Most of the team uses IntelliJ IDEA, but others IDEs can be used, for exemple the Eclipse IDE.

### Definition of Done

Before finishing a requirement, you need to check if everything is done for that particular requirement.
For that, we have a Definition of Done; the DoD decides when a requirement is really done.

Note: A Definition of Done is not a static list. It can be modified any time, if people feel like corrections should be made.

Current Definition of Done:
- Assumptions of requirements are met.
- Required documentation is done.
- (Software) Requirement is accepted and got a thumbs up from the Maintainer via an accepted Pull Request.
- (Software) The build succeeds without failures.
- (Software) All tests in the test suite pass.
- (Software) Code style checks report no violations.
- (Software) Code coverage is high enough (a minimum of 80% is covered).
- (Software) If applicable, the added Unit Test is written, executed and passed.
- (Software) Security analysis (vulnerability detection) doesn't spot unaddressed issues.

### How-to begin

Before you start your coding journey within the CoMPAS project, there are some things we have to talk about.
Some things that will make your start a little easier!
On the [developing](DEVELOPING.md) page information about tooling can be found.

#### Open Community Calls

It's good to know that every other monday, we are having a so called Open Community Call. Everyone participating in the CoMPAS project can join 
and talk about and ask question about the CoMPAS project.

When the Open Community Calls are taking place, can be found at the [General CoMPAS mailing list calendar](https://lists.lfenergy.org/g/CoMPAS/calendar).

The agendas can be found at the [LF Energy wiki](https://wiki.lfenergy.org/display/HOME/CoMPAS+Community+Calls).

If you have something to add, please add it to the agenda and notify everyone on Slack!

#### Slack channel

One of the first important things, is to meet the community. Feel free to introduce yourself by joining the channel 'compas' on [LF Energy Slack](https://slack.lfenergy.org/)!

The Slack channel is the first communication platform within the CoMPAS project (besides email and the Github platform), so if you need help for example you can use Slack!

#### Documenting
A good (open source) project requires documentation. We have two places for our documentation

##### LF Energy Wiki

LF Energy has it's own [CoMPAS specific Wiki](https://wiki.lfenergy.org/display/HOME/CoMPAS). This is the place for documenation 
about CoMPAS in general (like roadmap and the community call agendas).

#### Architecture and technologies

For all architecture and technology choices (for example frameworks, build tools, database choices, etcetera),
please check the source code (duh!) and our [CoMPAS Architecture Github Pages](https://com-pas.github.io/compas-architecture/).

### Copyright and Licensing

Copyright and license information is done on per-file basis. We use the specification of [REUSE](https://reuse.software/spec/)
to ensure that copyright information of the project is clear and can be analuzed in an automated fashion.

Every source code repository within CoMPAS has a Github Action for checking against the REUSE specification.

For more information, check the [Copyright Guidelines](STYLEGUIDE.md#copyright-guidelines) section.

### Github Project Boards

For managing the CoMPAS issues created in all the separate repositories, we use the [Projects Board](https://github.com/orgs/com-pas/projects) of Github.
CoMPAS uses 2 different Project Boards: One for [Pull Requests](https://github.com/orgs/com-pas/projects/2) and one for the [Issues](https://github.com/orgs/com-pas/projects/1).

When creating Pull Requests or Issues, it will automatically create issues or Pull Requests on the Project Boards.
This is done by the Automate Projects Github Action (take a look at the [action from the Data Service](https://github.com/com-pas/compas-core/blob/master/.github/workflows/automate-projects.yml) for example).
Changing the status of Issues / Pull Requests is also handled automatically by the Github Action.

Issues and Pull Requests can be moved on both the Project Boards and on the boards of the specific repository itself. It synchronizes automatically.

## Github Pages

This site is provided as a [github pages site](https://com-pas.github.io/contributing/).
The content is maintained and edited on [Github](https://github.com/com-pas/contributing) in the directory "docs".
Contributors are only allowed to contribute by editing the content on Github and must do so by presenting their modifications as *pull-request* to the community.
The diagrams on this page are created using [DrawIO](https://github.com/jgraph/drawio-desktop/releases)
and follow [Unified Modeling Language (UML)](https://www.omg.org/spec/UML/).
The drawIO design file is available on this site: [/blob-files/CoMPAS.drawio](blob-files/CoMPAS.drawio).
Modification made to UML diagrams on this site must be made in this file and the modified file must be part of the pull request.
