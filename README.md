# build-release-cicd-sample
This is an example of setting up CI/CD pipeline for a Java Maven project using GitHub Actions

# CI/CD with GitHub Actions 

This GitHub Actions workflow automates the build and release process for a Java Maven project. The workflow includes two jobs: "build" and "release," each serving a specific purpose in the Continuous Integration and Continuous Deployment (CI/CD) pipeline.

## Trigger

This workflow is triggered on each push to the main branch. Any changes pushed to the main branch will initiate the workflow, ensuring that the latest code changes are automatically tested, built, and released.

```
on:
  push:
    branches:
      - main
```

## Build Job
The "build" job is responsible for setting up the Java environment, compiling the project using Maven, and running any necessary build tasks. This job uses the `actions/setup-java` action to configure JDK 11.

```
jobs:
  build:
    runs-on: ubuntu-latest
    steps:
      - name: 'Set up JDK 11'
        uses: actions/setup-java@v3
        with:
          java-version: '11'
          distribution: 'temurin'

      - name: Build with Maven
        run: mvn clean install -Dmaven.test.skip=true --file pom.xml
```
### Explanation:

 - `runs-on`: Specifies the operating system on which the workflow will run (`ubuntu-latest` in this case).
 - `Set up JDK 11`: Configures the Java environment using JDK 11 from the Temurin distribution.
 - `Build with Maven`: Executes Maven commands to clean the project, install dependencies, and build the project. The `-Dmaven.test.skip=true` flag skips running tests during the build.

## Release Job

The "release" job is dependent on the successful completion of the "build" job. It uses the `softprops/action-gh-release` action to create a GitHub release, including the generated JAR files in the release assets.

```
  release:
    runs-on: ubuntu-latest
    needs: build
    steps:
      - name: Create Release
        uses: softprops/action-gh-release@v1
        with:
          files: target/*.jar
```

### Explanation:
 - `needs: build`: Specifies that the "release" job depends on the successful completion of the "build" job.
 - `Create Release`: Utilizes the `softprops/action-gh-release` action to create a GitHub release. The specified files (JAR files in the `target` directory) are included in the release as assets.

This workflow streamlines the software delivery process, providing a seamless CI/CD pipeline for Java Maven projects hosted on GitHub. Any commits to the main branch will automatically trigger these defined steps, ensuring that the project is continuously integrated, tested, and released with each push.