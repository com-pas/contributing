# Developing for CoMPAS

#### Table Of Contents

* [Tooling](#tooling)
* [IDEs](#ides)
* [GitHub Actions](#github-actions)
* [Others](#others)

## Tooling

### GitHub
What's GitHub? It's where you're looking right now! (Joking!).

We are using GitHub for hosting our Git repositories. GitHub is being used for creating issues and creating Pull 
Requests to review / merge each others code.

### LFX Security Tool
For checking potential security issues, we use the [LFX Security Tool](https://security.lfx.linuxfoundation.org/#/e8b6fdf9-2686-44c5-bbaa-6965d04ad3e1/licenses). 
The LFX Security Tool scans selected repositories for potential security issues in dependencies. 
It also scans every license that is being used within a repository and checks if they are compatible within open source projects.

### SonarCloud
CoMPAS is using [SonarCloud](https://sonarcloud.io/organizations/com-pas/projects) for static code analysis. 
Every GitHub repository has a GitHub Action which automatically pushes the code to SonarCloud with a frequency of the given 
GitHub Action (most of the time on each push).

A Pull Request can't be merged before all SonarCloud issues are being fixed!

### GitHub Packages
To make artifacts available between the different GIT repositories we are using GitHub Packages to distribute these.
Every GIT repository can build its artifacts and publish these to GitHub Packages.
Other GIT repositories can then add GitHub Packages as Maven repository to their build tool.
See below how to do both action for the specific tools.

To use GitHub Packages a username and token is needed. The username is your GitHub username. The token can be generate 
in GitHub by going to your settings, Developer settings, Personal access tokens.
Generate a new token here and make sure that the scope "read:packages" is enabled. Use this token below to configure the build tools.

### Basic Maven
The project uses Maven to manage the build. Most projects use multi-module structures to build all code. A basic command to run Maven is:
```
$ maven clean verify
```

#### GitHub Packages in Maven
To use GitHub Packages in Maven an extra repository need to be added to the build process.
```xml
<repositories>
    <repository>
        <id>github-packages-compas</id>
        <name>GitHub Packages CoMPAS</name>
        <url>https://maven.pkg.github.com/com-pas/*</url>
    </repository>
</repositories>
```
Because credentials are needed for GitHub Packages, these will be passed by using the Settings.xml file.

#### Maven Local Settings.xml for GitHub Packages
Edit (or create if not already exists) the `~/.m2/settings.xml` file and add the following content:
```xml
<settings xmlns="http://maven.apache.org/SETTINGS/1.0.0" 
          xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" 
          xsi:schemaLocation="http://maven.apache.org/SETTINGS/1.0.0
                              http://maven.apache.org/xsd/settings-1.0.0.xsd">
  
    <servers>
        <server>
            <id>github-packages-compas</id>
            <username>username</username>
            <password>password</password>
        </server>
    </servers>
  
</settings>
```

Add this server section. The ID of the server must be the same as the ID found in the previous repository ID, it should map.
Username should be your GitHub username, password can both be your own [encrypted password](https://maven.apache.org/guides/mini/guide-encryption.html)
or a [Personal Access Token](https://docs.github.com/en/github/authenticating-to-github/keeping-your-account-and-data-secure/creating-a-personal-access-token).

## IDEs
If your IDE is supported by sonarlint (both IntelliJ IDEA and the Eclipse IDE are supported), it is recommended to install it. 
It provides immediate feedback on most sonar issues. Running tests individually is often possible in IDEs without invoking maven. 
Please consult the documentation of your IDE for setting up the project through maven integration.

For MapStruct there are plugins available for both IDEs mentioned below. See this [page](https://mapstruct.org/documentation/ide-support/).

### Intellij IDEA
Import the project using IDEA's maven integration in the GUI. Install SonarLint. Code!

### Eclipse IDE
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

## GitHub Actions

### Settings.xml during GitHub Action for GitHub Packages
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

## Releasing software

### Create a release

To create a release of the software we are using the release functionality of GitHub. Under the tab 'code' there is a section
'Releases'. When selected all current releases will be displayed, and a new release can be created (draft release).
The standard branch to create a release from should be the "main" branch, so the code changes have been reviewed.

For every repository that creates a software product (artifacts or docker images) a GitHub Action ('release-project.yml') 
is defined. This action runs when a release is created. 
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

### Publish artifacts using Maven

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
The ID is the same as the ID used for the repository section [above](#github-packages-in-maven).
This way the same credentials will be used to connect to GitHub Packages as described [above](#maven-local-settingsxml-for-github-packages).
Replace "[repo-name]" with the name of the repository from CoMPAS.

## Others

### Adding custom badges to your README
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
