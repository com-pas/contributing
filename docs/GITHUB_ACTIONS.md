# GitHub Actions
Every repository within the CoMPAS GitHub organization need a default set of GitHub Actions.
[GitHub Actions](https://github.com/features/actions) are CI/CD steps within a GitHub Repository that you can configure. This way, you can ensure that certain steps (like building) are always triggered on for example a commit push.

Within CoMPAS, we define the following 'must have' GitHub Actions:
  - [Building](#building)
  - [REUSE check](#reuse-check)
  - [SonarCloud](#sonarcloud)
  - [Docker Hub Deployment](#docker-hub-deployment)

More to follow.

GitHub Actions are configured using YAML files. These files are stored in the `.github/workflows` directory of a specific repository.

## Maven settings.xml during GitHub Action for GitHub Packages
During multiple GitHub Actions (like building and SonarCloud analysis), the custom `settings.xml` file is needed because it needs access to the GitHub Packages
to download certain artifacts. We can do this by adding the following step **before** the GitHub Packages repository is needed.

```yaml
- name: Create custom Maven Settings.xml
  uses: whelk-io/maven-settings-xml-action@v18
  with:
    output_file: custom_maven_settings.xml
    servers: '[{ "id": "github-packages-compas", "username": "OWNER", "password": "${{ secrets.GITHUB_TOKEN }}" }]'
```

This basically creates a custom `settings.xml` at location `custom_maven_settings.xml`. This file can be passed to maven in the next step
by using `mvn -s custom_maven_settings.xml` and perhaps some extra parameters you wish for.

For the `servers` part, we again have the `github-packages-compas` ID that needs to be the same. We have an `OWNER` username (this is the default, because
it needs to have a username) and a password which is the GITHUB_TOKEN that's always available.

## Building
All source code repositories need some kind of building step.
By default, all source code repositories use Maven as the build tool.

This building step is pretty easy to configure. Just create a `maven_build.yml` file in the `.github/workflows` directory containing the following source code:

```yaml
name: Maven Build

on: push #(1)

jobs:
  build:
    name: Build
    runs-on: ubuntu-latest
    timeout-minutes: 15

    steps:
      - uses: actions/checkout@v2
      - name: Set up JDK 1.11
        uses: actions/setup-java@v2.3.0
        with:
          distribution: 'zulu'
          java-version: '11'
      - name: Create custom Maven Settings.xml #(2)
        uses: whelk-io/maven-settings-xml-action@v18
        with:
          output_file: custom_maven_settings.xml
          servers: '[{ "id": "github-packages-compas", "username": "OWNER", "password": "${{ secrets.GITHUB_TOKEN }}" }]'
      - name: Build with Maven
        run: mvn -s custom_maven_settings.xml -B clean verify #(3)
```

A few points to remember:
- (1): By default, all actions are triggered on a push action.
- (2): This step creates a custom settings.xml for authenticating for GitHub Packages. For more information,
  check the [Contributing file](https://github.com/com-pas/contributing/blob/master/CONTRIBUTING.md).
- (3): This is a default for building a Maven project. It may differ in some cases, please feel free to adjust it.

## REUSE check
For keeping our copyright and licensing information up to date and correct, we use [REUSE](https://reuse.software/) to check this. This is also configured for every separate repository in an easy manner: just create a `reuse.yml` file in the `.github/workflows` directory containing the following source code:

```yaml
name: REUSE Compliance Check

on: push #(1)

jobs:
  test:
    runs-on: ubuntu-latest
    steps:
    - uses: actions/checkout@v2
    - name: REUSE Compliance Check
      uses: fsfe/reuse-action@v1
```

A few points to remember:
- (1): By default, all actions are triggered on a push action.

This is the only thing that has to be done. After this, it will be checked on every push.

### REUSE badge
For transparency, CoMPAS repositories also include a REUSE badge in their README for fast checking the REUSE compliance.

Two steps are needed to get a REUSE badge to work:

1. Register the Repository at the [REUSE website](https://api.reuse.software/register). For name and email, check the [Slack channel](https://app.slack.com/client/TLU68MTML).
2. Add the following code to the README:
   
```md
[![REUSE status](https://api.reuse.software/badge/github.com/com-pas/repoName)](https://api.reuse.software/info/github.com/com-pas/repoName)
```

There is one steps left: Replace `repoName` with the name of the specific repository (as stated in the URL).

After doing all these steps, everything is set up for the REUSE check.

## SonarCloud
For static code analysis, CoMPAS is using [SonarCloud](https://sonarcloud.io/). To configure this, there are several steps that needs to be done.

1. Go to the [CoMPAS GitHub organization settings](https://github.com/organizations/com-pas/settings/profile), and click on "Installed GitHub Apps". SonarCloud is listed here already (because we are already using it). Click on the 'configure' button next to it.
2. In the "Repository access" section, select the repository you want to add. By default, not every repository is added as an extra check.
3. Create a new project in [SonarCloud](https://sonarcloud.io/projects/create).
4. Select the repository to be analyzed, click Set Up.
5. Choose the Analysis Method "With GitHub Actions".
6. It first tells you to create a SONAR_TOKEN secret in your repo. Go to your repository -> Settings - Secrets -> New repository secret -> Name: SONAR_TOKEN. Value: Copy the value from the SonarCloud website into here. Then save the secret
7. Select Maven as the option that best describes our build and **remember the projectKey**. and create a `sonarcloud_analysis.yml` file in the `.github/workflows` directory containing the following source code running.

```yaml
name: SonarCloud Analysis

on: push #(1)

jobs:
  build:
    name: Build
    runs-on: ubuntu-latest
    timeout-minutes: 15

    steps:
      - uses: actions/checkout@v2
        with:
          fetch-depth: 0
      - name: Set up JDK 1.11
        uses: actions/setup-java@v3.9.0
        with:
          distribution: 'zulu'
          java-version: '11'
          cache: 'maven'
      - name: Cache SonarCloud packages
        uses: actions/cache@v2.1.6
        with:
          path: ~/.sonar/cache
          key: ${{ runner.os }}-sonar
          restore-keys: ${{ runner.os }}-sonar
      - name: Create custom Maven Settings.xml #(2)
        uses: whelk-io/maven-settings-xml-action@v18
        with:
          output_file: custom_maven_settings.xml
          servers: '[{ "id": "github-packages-compas", "username": "OWNER", "password": "${{ secrets.GITHUB_TOKEN }}" }]'
      - name: Build and analyze
        env:
          SONAR_TOKEN: ${{ secrets.SONAR_TOKEN }}
        run: | #(3)
          mvn -s custom_maven_settings.xml -B -Psonar \
          -Dsonar.projectKey=<insert project key> \
          -Dsonar.organization=com-pas \
          -Dsonar.host.url=https://sonarcloud.io \
          verify org.sonarsource.scanner.maven:sonar-maven-plugin:sonar
```

A few points to remember:
- (1): By default, all actions are triggered on a push action.
- (2): Creates a custom `settings.xml` having the credentials for the GitHub Packages.
  For more information, check our [Contributing](https://github.com/com-pas/contributing/blob/master/CONTRIBUTING.md).
- (3): Replace the `<insert project key>` with the project key you copied.

Once this is set, it's all done!

## Docker Hub Deployment
For automatic deployment of our microservices, CoMPAS uses Docker Hub as the central docker image repository. This way, all Docker images can be pulled from a central image repository.

This step is easy to configure. Just create a `dockerhub_deployment.yml` file in the `.github/workflows` directory containing the following source code:

```yaml
name: Docker Hub Deployment

on:
  release:
    types: [released] #(1)

jobs:
  push_to_registry:
    name: Build and publish
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v2
      - name: Login to Docker Hub #(2)
        uses: docker/login-action@v1
        with:
          username: ${{ secrets.DOCKER_HUB_USERNAME }}
          password: ${{ secrets.DOCKER_HUB_PASSWORD }}
      - name: Extract tag name #(3)
        id: extract_tagname
        shell: bash
        # Extra the tagname form the git reference, value of GITHUB_REF will be something like refs/tags/<tag_name>.
        run: echo "##[set-output name=tagname;]$(echo ${GITHUB_REF##*/})"
      - name: Set up JDK 11
        uses: actions/setup-java@v2.3.0
        with:
          distribution: 'zulu'
          java-version: '11'
      - name: Create custom Maven Settings.xml
        uses: whelk-io/maven-settings-xml-action@v18
        with:
          output_file: custom_maven_settings.xml #(4)
          servers: '[{ "id": "github-packages-compas", "username": "OWNER", "password": "${{ secrets.GITHUB_TOKEN }}" }]'
      - name: Set version with Maven
        run: mvn -B versions:set -DprocessAllModules=true -DnewVersion=${{ steps.extract_tagname.outputs.tagname }}
      - name: Deploy with Maven to GitHub Packages and Docker Hub #(5)
        run: ./mvnw -B -s custom_maven_settings.xml -Prelease,native clean deploy
```

A few points to remember:
- (1): By default, the docker image is only deployed on release. This way we have a strict deployment flow, and versions always have the same content. For more information about types of releases, check the [GitHub Actions documentation](https://docs.github.com/en/actions/reference/events-that-trigger-workflows#release).
- (2): Before deploying to Docker Hub, we need to login first. This can be done by executing a GitHub Action. In this example, `DOCKER_HUB_USERNAME` and `DOCKER_HUB_PASSWORD` are used, which are secrets stored at [CoMPAS organization](https://github.com/organizations/com-pas/settings/secrets/actions). For more information about the username and password, ask in the the [Slack channel](https://app.slack.com/client/TLU68MTML).
- (3): Extract the tag name from Git and use that as version to be set with Maven (${{ steps.extract_tagname.outputs.tagname }}).
- (4): Creates a custom `settings.xml` having the credentials for the GitHub Packages.
  For more information, check our [Contributing](https://github.com/com-pas/contributing/blob/master/CONTRIBUTING.md).
- (5): Building and publishing the docker image is build tool / framework specific. By default, CoMPAS services are using Quarkus and Maven. Deploying to Docker Hub is quite easy using Quarkus and maven, it's just a matter of building in combination with setting some properties. In this example, we use the `quarkus-profile` parameter instead of including all the parameters. This way, we can define profile specific properties in our `application.properties` file (For more information about this, check our [Docker Hub Deployment page](./DEPLOYMENT.md)):

```ini
%publishNativeImage.quarkus.native.container-build=true
%publishNativeImage.quarkus.container-image.build=true
%publishNativeImage.quarkus.container-image.group=lfenergycompas
%publishNativeImage.quarkus.container-image.name=compas-scl-data-service
%publishNativeImage.quarkus.container-image.push=true
```
