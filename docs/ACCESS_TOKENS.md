# Access Tokens

In all parts of the development process we are using Tokens to access other systems then GitHub. To connect to these
systems we often use Access Token to authorize access. These access tokens are personal access token from members of
the CoMPAS Organization. To keep track we keep a list of access token used and who created the access token. This way 
we know when they expire and if someone leaves the CoMPAS Organization which ones to replace.

## Overview Access Tokens

Below tokens are al registered under the CoMPAS Organisation as Action Secret.

| Token Name               | Owner           | Expire Date |
|--------------------------|-----------------|-------------|
| DOCKER_HUB_USERNAME      | LF Energy User  | -           |
| DOCKER_HUB_TOKEN         | LF Energy User  | -           |
| ORG_GITHUB_ACTION_SECRET | Dennis Labordus | 31/12/2022  |
| SONAR_TOKEN              | CoMPAS General  | -           |

Below tokens are al registered under the CoMPAS Organisation as Dependabot Secret.

| Token Name         | Owner           | Expire Date |
|--------------------|-----------------|-------------|
| DB_GITHUB_PACKAGES | Dennis Labordus | 31/12/2022  |

Currently, there are no GIT Repository specific secrets registered. Some access tokens are limited to a list of GIT 
Repositories allowed to use it.
