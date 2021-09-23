# Dependabot

## Configure Dependabot Settings

The first step is that we want to enable Alerts from Dependabot. This needs to be done for every repository that 
uses Dependabot. Go to the settings of the repository and select "Security & analysis". Next enable "Dependabot alerts"
and "Dependabot security updates".

Also remember to give the repository access to the secret DB_GITHUB_PACKAGES at organization level.

## Configure Dependabot (yaml)

Next step is to add a configuration file to the repository that will enable Dependabot. In the repository create a 
file "dependabot.yml" in the directory ".github". The basic content will be something like

```yaml
version: 2

registries: #(1)
  maven-github:
    type: maven-repository
    url: https://maven.pkg.github.com/com-pas/*
    username: OWNER
    password: ${{ secrets.DB_GITHUB_PACKAGES }}

updates:
  # Maintain dependencies for GitHub Actions
  - package-ecosystem: "github-actions" #(2)
    directory: "/"
    schedule:
      interval: "daily"
    open-pull-requests-limit: 5

  # Maintain dependencies for Maven
  - package-ecosystem: "maven" #(3)
    directory: "/"
    registries:
      - maven-github
    schedule:
      interval: "daily"
    open-pull-requests-limit: 5
```

- (1): This configures the GitHub Maven Repository that Dependabot can use to search for new versions of CoMPAS Dependencies.
- (2): This will check if the newest versions of GitHub Actions are used. Default it will scan the directory ".githuib/workflows" 
       under the directory defined here (in this case "/").
- (3): This will scan the Maven Plugins and Dependencies defined in the pom.xml files. The GitHub Maven Repository is also
       defined here.

Both scans are executed daily and create a maximum of 5 pull request. After a pull request is handled it will create the 
next one if there is one.

The configuration can be fine-tuned further. For instance with some libraries it isn't possible to use the latest version.
Below is an example how to prevent Dependabot from creating pull request for these dependencies. In the example 
the JAXB Implementation can't higher.

```yaml
  # Maintain dependencies for Maven
  - package-ecosystem: "maven"
    directory: "/"
    registries:
      - maven-github
    schedule:
      interval: "daily"
    open-pull-requests-limit: 5
    ignore:
      # Next two dependencies shouldn't be upgrade, because RestEasy isn't using newer version. (2.3.X)
      - dependency-name: jakarta.xml.bind:jakarta.xml.bind-api
        versions: [ "[3.0,)" ]
      - dependency-name: com.sun.xml.bind:jaxb-impl
        versions: [ "[3.0,)" ]
```

Dependabot uses the configuration found in the default branch (often 'develop'), so to make it effective use a 
pull request to merge it into the default branch.

## Adding Dependabot Secret DB_GITHUB_PACKAGES

Tot access GitHub Packages a secret DB_GITHUB_PACKAGES needs to be created.
- First create a new personal access token from https://github.com/settings/tokens. Tokens can only be created as personal tokens.
  The token also must have the right "read:packages".
- Next create a new organisation secret from https://github.com/organizations/com-pas/settings/secrets/dependabot with the value of
  personal access token you created above. Name the secret DB_GITHUB_PACKAGES. Make it available to the repositories that have
  Dependabot configured.

Now Dependabot can use this secret to access GitHub Packages.

## Handling the pull request

Pull request created by Dependabot can be handled just like other pull request, but there is 1 issue to know.  
Some GitHub Actions, like SonarCloud and AutomateProjects, will fail if they are started by the pull request from
Dependabot. This is caused by a security issues that was fixed. These actions can't access Secrets when started by a Bot.  
For some of these actions it maybe solved in some way, but if that is not possible just manually re-run the action.
The action will then succeed. 
