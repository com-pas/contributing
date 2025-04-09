# Developing for CoMPAS

#### Table Of Contents

* [CoMPAS components](#compas-components)
* [Local development](#local-development)
* [Tooling](#tooling)
* [IDEs](#ides)
* [GitHub Actions](#github-actions)
* [Others](#others)

## CoMPAS components

CoMPAS is composed of multiple components, each designed to fulfill a specific role within the project. The following is an overview of these components, their description, and their associated repositories.

- **CoMPAS Open SCD**  
  *Description*: Provides a web-based tool for editing and validating Substation Configuration Description (SCD) files.  
  *Technologies*: TypeScript, JavaScript  
  *Repository*: [https://github.com/com-pas/compas-open-scd](https://github.com/com-pas/compas-open-scd)

- **CoMPAS SCL Data Service**  
  *Description*: Manages SCL data storage and retrieval.  
  *Technologies*: Java, Quarkus, Maven, PostgreSQL  
  *Repository*: [https://github.com/com-pas/compas-scl-data-service](https://github.com/com-pas/compas-scl-data-service)

- **CoMPAS CIM Mapping**  
  *Description*: Handles mapping between IEC CIM and IEC 61850.  
  *Technologies*: Java, Quarkus, Maven  
  *Repository*: [https://github.com/com-pas/compas-cim-mapping](https://github.com/com-pas/compas-cim-mapping)

- **CoMPAS SCL Validator**  
  *Description*: Validates SCL files to ensure compliance with standards.  
  *Technologies*: Java, Quarkus, Maven  
  *Repository*: [https://github.com/com-pas/compas-scl-validator](https://github.com/com-pas/compas-scl-validator)

- **CoMPAS SCL Auto Alignment**  
  *Description*: Automatically aligns SCL files without coordinates.  
  *Technologies*: Java, Quarkus, Maven  
  *Repository*: [https://github.com/com-pas/compas-scl-auto-alignment](https://github.com/com-pas/compas-scl-auto-alignment)

- **CoMPAS Core**  
  *Description*: Provides core utilities and shared functionality for other CoMPAS services.  
  *Technologies*: Java, Quarkus, Maven  
  *Repository*: [https://github.com/com-pas/compas-core](https://github.com/com-pas/compas-core)

- **CoMPAS SCT**  
  *Description*: A library for generating SCD files based on the IEC 61850 standard.  
  *Technologies*: Java, Quarkus, Maven  
  *Repository*: [https://github.com/com-pas/compas-sct](https://github.com/com-pas/compas-sct)

- **CoMPAS SCL XSD**  
  *Description*: Contains the official IEC 61850-6 SCL XSD schemas.  
  *Technologies*: Java, Quarkus, Maven  
  *Repository*: [https://github.com/com-pas/compas-scl-xsd](https://github.com/com-pas/compas-scl-xsd)

- **CoMPAS Deployment**  
  *Description*: Provides tools and configurations for deploying CoMPAS services.  
  *Repository*: [https://github.com/com-pas/compas-deployment](https://github.com/com-pas/compas-deployment)

- **CoMPAS Architecture**  
  *Description*: Contains architectural documentation and diagrams for the CoMPAS project.  
  *Repository*: [https://github.com/com-pas/compas-architecture](https://github.com/com-pas/compas-architecture)

## Local development

This section provides instructions for setting up a local development environment and running the CoMPAS stack or individual services.

### Prerequisites

Ensure the following tools are installed on your system:
- [Docker](https://www.docker.com/)
- [Docker Compose](https://docs.docker.com/compose/)

### Running the CoMPAS stack locally

To run the entire CoMPAS stack locally, you can use the [compas-deployment](https://github.com/com-pas/compas-deployment) repository, which provides a pre-configured `docker-compose-postgresql.yml` file. Follow these steps:

1. Clone the `compas-deployment` repository:

```bash
git clone https://github.com/com-pas/compas-deployment.git
cd compas-deployment
```

2. To build and start all services run:

```bash
docker-compose --env-file compas/.env -f compas/docker-compose-postgresql.yml up -d --build
```

3. Use the `./bin/docker-wait-on-containers.sh` script to wait for all containers to be fully initialised. Once all containers are ready, it will print a message indicating that no containers are in the health=starting state.

```bash
./bin/docker-wait-on-containers.sh
```

4. Once the stack is running, the application will be available at [http://localhost](http://localhost). You can log in using the credentials configured in the Keycloak demo setup. For more information on the Keycloak users that are created, refer to the [Keycloak Demo Configuration documentation](https://github.com/com-pas/compas-deployment/tree/main?tab=readme-ov-file#keycloak-demo-configuration). If you encounter any issues, refer to the [Known Issues section](https://github.com/com-pas/compas-deployment/tree/main?tab=readme-ov-file#known-issue-with-docker-compose) in the compas-deployment repository.

5. To stop the stack, run:

```bash
# Stop all containers
docker-compose -f compas/docker-compose-postgresql.yml down
```
or

```bash
# Stop all containers and remove the volumes.
docker-compose -f compas/docker-compose-postgresql.yml down -v
```

### Running an individual service locally

Each repository contains the relevant and most up-to-date information on setting up the development environment and running the service locally. Refer to the `DEVELOPMENT.md` file or equivalent documentation in the respective repository for detailed instructions.

| Component                     | Development Guide                                                                                     |
|-------------------------------|-------------------------------------------------------------------------------------------------------|
| **CoMPAS Open SCD**           | [Development Guide](https://github.com/com-pas/compas-open-scd/blob/main/packages/compas-open-scd/DEVELOPMENT.md) |
| **CoMPAS SCL Data Service**   | [Development Guide](https://github.com/com-pas/compas-scl-data-service/blob/main/doc/postgresql.md)      |
| **CoMPAS CIM Mapping**        | [Development Guide](https://github.com/com-pas/compas-cim-mapping/blob/main/DEVELOPMENT.md)           |
| **CoMPAS SCL Validator**      | [Development Guide](https://github.com/com-pas/compas-scl-validator/blob/main/DEVELOPMENT.md)         |
| **CoMPAS SCL Auto Alignment** | [Development Guide](https://github.com/com-pas/compas-scl-auto-alignment/blob/main/DEVELOPMENT.md)    |
| **CoMPAS Core**               | Not available                                                                                         |
| **CoMPAS SCT**                | Not available                                                                                         |
| **CoMPAS SCL XSD**             | Not available                                                                                        |
| **CoMPAS Deployment**         | [Development Guide](https://github.com/com-pas/compas-deployment/blob/main/README.md)           |

## Tooling

### GitHub
What's GitHub? It's where you're looking right now! (Joking!).

We are using GitHub for hosting our Git repositories. GitHub is being used for creating issues and creating Pull 
Requests to review / merge each other's code.

#### GitHub User Settings
We noticed that there are sometimes problems with the DCO Check. This can be caused by an email setting of the user. 
It's about keeping the email address private. 

Go to Settings of your account and select the option `Emails`. Here you will find the setting `Keep my email addresses 
private`. Uncheck this setting. When checked there are cases that the DCO Check will fail, because the `github.com` 
email address is used in the Web interface. When failing it will cause a lot of work to fix it.

### LFX Security Tool
For checking potential security issues, we use the [LFX Security Tool](https://security.lfx.linuxfoundation.org/#/a092M00001IkJTLQA3/overview). 
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

To use GitHub Packages a username and token is needed. The username is your GitHub username. The token can be generated 
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
If your IDE is supported by SonarLint (both IntelliJ IDEA and the Eclipse IDE are supported), it is recommended to install it. 
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
- Choose a color and a pre- or suffix text.
