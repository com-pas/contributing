# Releasing software

## Create a release

### Backend services

We are using [release please](https://github.com/googleapis/release-please) which automatically creates and updates a release PR. The PR contains an up to date changelog based on the conventional commit messsages and sets the new version number. To trigger a release the release please PR has to be merged, which triggers the release please workflow (currently the release please PR has to be merged twice, because it creates a snapshot version first). The workflow will build the Maven packages, upload them to GitHub packages and build the Docker image and upload it to Docker hub.

### Compas open scd

In the compas-open-scd repository, update the version number in packages/compas-open-scd/package.json and make sure the submodule pointers are correct. Go to https://github.com/com-pas/compas-open-scd. Under releases click Draft a new release. In Choose a tag, create a new tag using the new version number. Click Generate release notes — review the notes and ensure they’re correct.

Publishing the release will trigger a Github workflow and build the Docker image and upload it to Docker hub.

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

During creating of a release release please will automatically generate the release notes using commit messages.
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
