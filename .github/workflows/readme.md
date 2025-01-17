# Continuous Integration Final Project
## By: Cesar Illanes

The project already came with a pipeline called maven-build.yml that included:

```
# This workflow will build a Java project with Maven, and cache/restore any dependencies to improve the workflow execution time
# For more information see: https://help.github.com/actions/language-and-framework-guides/building-and-testing-java-with-maven

name: Java CI with Maven

on:
  push:
    branches: [ main ]
  pull_request:
    branches: [ main ]

jobs:
  build:

    runs-on: ubuntu-latest

    steps:
      - uses: actions/checkout@v2
      - name: Set up JDK 11
        uses: actions/setup-java@v2
        with:
          java-version: '11'
          distribution: 'adopt'
          cache: maven
      - name: Build with Maven Wrapper
        run: ./mvnw -B package
```

I changed the file name to main-workflow.yml and its content to meet the requirements.
* Added the ```./mvnw run``` command\
* Signed in into SonalCloud, create an organization and then added this repository as a project. It automatically scanned as you can see in the next screenshot.
  * As you can see in the picture, it recommends to change to the CI-based analysis, so that was my next step.
<img width="1042" alt="sonarcloud-automatic-screenshot" src="./screenshots/sonarcloud-automatic.png">

* I set-up the SONAR_TOKEN secret in GitHub Actions, added properties in pom.xml, added some lines to the workflow and disabled the automatic analysis.
* The sonarcloud-bot replied on the pull request made for the test and analysis job.
<img width="1042" alt="sonarcloud-bot-screenshot" src="./screenshots/sonarcloud-bot-comment.png">

* The quality gate showed that it passed.
<img width="1042" alt="sonarcloud-quality-gate" src="./screenshots/sonarcloud-quality-gate.png">
<img width="1042" alt="sonarcloud-quality-gate" src="./screenshots/sonarcloud-quality-gate-2.png">

* Here's the Quality Gate settings:
<img width="1042" alt="sonarcloud-quality-gate-settings" src="./screenshots/sonarcloud-quality-gate-settings.png">

* After that, I spent most of the time given into making work JFrog Artifactory. The steps I made were:
  * Try to set-up the virtual repository, it gave me a settings.xml file, that is a configuration file to point the artifactory servers and my username and password. It was needed to move to ~/.m2/ folder.
  * Add the distributionManagement tag, in order that Maven knows which is the place to upload the build.
  * After that, I couldn't upload the artifacts and I never realized why, in the pipeline it gave me HTTP 401 and 403 errors, made a lot of reserach on the internet and nothing happened.
* After giving up on JFrog, I integrated the Gitleaks (zricethezav/gitleaks-action@master) and the Snyk (snyk/actions/maven@master) actions. However, I could make work the Snyk, so I just commented it in the workflow.
