# Releasing software

## Create a release

To create a release of the software we are using the release functionality of GitHub. Under the tab ``code`` there is a section
``Releases`` (Right side). When selected all current releases will be displayed, and a new release can be created (draft release).
The standard branch to create a release from should be the ``main`` branch.

Enter the following values when creating a new release:
- **Choose a tag**: Enter a new version using semantic versioning, for example ``0.1.4``. 
  Also press ``Create new tag`` to create the new label.
- **Target**: This should normally be ``main``.
- **Release title**: Name of the release, use the following template ``Release <tag> of <project name>``, for instance 
  ``Release 0.1.4 of SCL Auto Alignment Service``
- **Describe the release**: These are the release notes, press the button ``Auto-generate release notes`` to generate these.
  Check [Configure release notes generation](#configure-release-notes-generation) to configure how these are generated.

Now press ``Publish release`` to create the release. For every repository that creates a software product (artifacts or docker images) 
a GitHub Action (``release-project.yml``) is defined. This action runs when a release is created.
```yaml
on:
  release:
    types: [released]
```

Depending on the type of project different steps will be executed.
Common steps are:
- Checking out the source code,
- Extracting the entered version from the Git Tag.
- Set version using Maven
- Setup Maven settings.xml file

Depending on the type of project other steps will be executed. Some examples are:
- Build and publish the software to GitHub Packages using Maven
- Build and publish the docker image to DockerHub using Maven
- Build and publish the docker image to DockerHub using NPM and Docker

## Publish artifacts using Maven

To publish artifacts to GiHub Packages a distribution section needs to be added to pom.xml of the root.
```xml
<distributionManagement>
    <repository>
        <id>github-packages-compas</id>
        <name>GitHub Packages</name>
        <url>https://maven.pkg.github.com/com-pas/[repo-name]</url>
    </repository>
</distributionManagement>
```
The ID is the same as the ID used for the repository section in [Maven Build](DEVELOPING.md#github-packages-in-maven).
This way the same credentials will be used to connect to GitHub Packages as described [Maven Build](DEVELOPING.md#maven-local-settingsxml-for-github-packages).
Replace ``[repo-name]`` with the name of the repository from CoMPAS.

## Configure release notes generation

During creating of a release we will use the GitHub feature to automatically generate the release notes using the pull requests.
The way these release notes are created can be configured by adding/updating the file ``release.yml`` to the directory ``.github``.

The content of the file ``release.yml`` is currently:
```yaml
changelog:
  exclude:
    labels:
      - wontfix
      - duplicate
      - invalid
  categories:
    - title: New Features
      labels:
        - enhancement
    - title: Bugfixes
      labels:
        - bug
    - title: Tooling changes
      labels:
        - tooling
    - title: Dependency updates
      labels:
        - dependencies
    - title: Other Changes
      labels:
        - "*"
```

This will group different pull request using the different labels.
