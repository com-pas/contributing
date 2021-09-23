<!--
SPDX-FileCopyrightText: 2021 Alliander N.V.

SPDX-License-Identifier: CC-BY-4.0
-->

## Deployment of CoMPAS
We deploy the (native) Docker image of all CoMPAS services to Docker Hub. This way, it can be pulled and deployed into environments of your choice (OpenShift for example).

### Quick Deployment instructions (under construction)
The following instructions are terminal instructions for publishing a Quarkus docker image to Docker Hub. This should be done by a Github Action in the future.

```
docker login # Creating a Docker session
```
In order to publish it to Docker Hub, you have to login first and create a session.

```
./gradlew clean build -Pnative -Dquarkus.native.container-build=true -Dquarkus.container-image.build=true -Dquarkus.container-image.group=group -Dquarkus.container-image.name=name -Dquarkus.container-image.push=true
```
Few points in this single command:
- `-Pnative` creates a native executable (this needs some prework, see the Building Native Image [documentation](https://quarkus.io/guides/building-native-image)).
- `-Dquarkus.native.container-build=true` creates a Linux executable without GraalVM being installed (only necessary if GraalVM is not installed).
- `-Dquarkus.container-image.build=true` creates a container image with the native executable in it.
- `-Dquarkus.container-image.group=group` defining the group name. Standard it's the system user name.
- `-Dquarkus.container-image.name=name` defining the name of the image. Standard it's 'app'.
- `-Dquarkus.container-image.push=true` deploys to docker image to Docker Hub.

## Sources
#### Full documentation about deploying Quarkus application to Docker Hub
https://dev.to/marcuspaulo/tutorial-publish-a-quarkus-application-in-kubernetes-minikube-and-dockerhub-36nd