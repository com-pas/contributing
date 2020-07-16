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

[Styleguides](#styleguides)
  * [Git Commit Messages](#git-commit-messages)
  * [Java Styleguide](#java-styleguide)

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

#### Basic Maven Usage
The project uses maven to manage the build. The configuration of all the tools is fairly standard, so if you have already contributed to Java projects, you should feel right at home. You can safely run the full test suite, checkstyle, see code coverage information and the generated documentation with the following command:
```
$ mvn clean verify javadoc:aggregate -Pjacoco
```
You can then inspect global code coverage in `distribution-core/target/site/jacoco-aggregate/index.html` and the javadoc in `./target/site/apidocs/index.html`.

To run a full sonarqube analysis, download and launch a sonarcube server on your machine and add the `sonar:sonar` goal at the end of the maven command line. However, it is not practical to run it every time. A more efficient alternative is to run sonarlint in an IDE.

#### Advanced Maven Usage

Using maven, you can selectively run some tests and compile only some projects and avoid long package generation tasks. Here are a few examples that are useful, feel free to compose them to find a workflow that suits you. Please consult Maven's documentation for more uses of maven.
```
# Simple run of a single test class in one project. Use either a colon followed by the ArtifactId or the directory of the project.
$ mvn test -Dtest=MyTestClass -DfailIfNoTests=false -pl :artifactid -am
# Simple run of a all tests in one project (use the correct package and artifactid)
$ mvn test -Dtest=MyProjectPackage.** -DfailIfNoTests=false -pl :artifactid -am
# Complex selection of tests
$ mvn test -Dtest=MyProject1Class#method1,MyProject1Class#method3,MyProjectPackage2.** -DfailIfNoTests=false -pl :artifact1,directory2 -am
# Sometimes problems are masked by maven trimming stacktraces:
$ mvn test -DtrimStackTrace=false
# Running tests and compiles in parallel can speed things up:
$ mvn -T 2.0C test
```

#### IDEs
If your IDE is supported by sonarlint (both IntelliJ IDEA and the Eclipse IDE are supported), it is recommended to install it. It provides immediate feedback on most sonar issues. Running tests individually is often possible in IDEs without invoking maven. Please consult the documentation of your IDE for setting up the project through maven integration.

##### Intellij IDEA
Import the project using IDEA's maven integration in the GUI. Install SonarLint. Code!

##### Eclipse IDE
Eclipse IDE has two ways to import maven projects: the eclipse GUI component m2e that understands maven or the maven CLI component maven-eclipse-plugin.

Using maven-eclipse-plugin, it is possible to recreate all the necessary eclipse files from scratch. A practical way to use it and get deterministic results is to remove all existing eclipse files, delete all eclipse projects from the workspace, regenerate all the eclipse files and reimport everything into eclipse as "existing eclipse projects". If all your projects are checked out outside of the eclipse workspace on the file system, then deleting all the projects is even simpler because you can just delete the whole workspace. The whole cycle takes only a few seconds.
```
# Ensure that eclipse is not running
# delete the projects in the GUI, or "rm -rf" all eclipse projects from the workspace
# delete all eclipse files from the file system
$ find . -name .project -o .classpath -o -name .settings -exec rm -rf '{}' \;
# regenerate eclipse files
$ mvn package eclipse:eclipse
# import as existing eclipse projects in the GUI (alternatively, use eclim's :ProjectImportDiscover directly from ViM)
```

After importing the projects with either method, install SonarLint for quicker feedback on potential sonar issues.

## Styleguides

### Git Commit Messages

As usual, please start the commit message with a short line describing the commit, then leave a blank line, then give more context and explanations. You can use GitHub's integrations, for exemple to link to existing issues. In general, pull requests with more than one commits will be squashed when merged in master.

### Java StyleGuide

- The project uses modern java, feel free to use any new APIs provided by the current java version (currently java 8).
- New API classes and methods should be documented with javadoc. Write higher level documentation for classes and lower level documentation for methods. For example, ...
- User-facing configuration options and general design decisions should be documented (where?)
- We use standard configurations of well known tools like checkstyle and sonarqube to enforce a coherent coding style, please consult those tools for justifications on these rules.

As a simple yet instructive example, consider ...
```java
/**
 * Example?
 */
```

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
