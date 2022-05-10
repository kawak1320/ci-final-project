<div align="center">
  <h1>Martín Pavesio Continuous Integration Final Project</h1>
</div>
# Table of Contents
[2.1 Source code](#source-code)
[2.1 Unit Tests](#tests)

## <a name="source-code"></a>2.1 Source code

### Added Project to the Repo
![alt text](images/Screenshot_20220509_215558.png?raw=true)

### Select Java with Maven Action
![alt text](images/Screenshot_20220509_220156.png?raw=true)
![alt text](images/Screenshot_20220509_220231.png?raw=true)

## <a name="tests"></a>2.1 Unit Tests
### Adding Unit Tests Job to the workflow

```yml
  tests:
    name: Unit Tests
    needs: [ gitleaks, snyk ]
    runs-on: ubuntu-latest

    steps:
      - uses: actions/checkout@v1
      - name: Set up JDK 11
        uses: actions/setup-java@v1
        with:
          java-version: '11'

      #Set up Maven cache
      - name: Cache Maven packages
        uses: actions/cache@v1
        with:
          path: ~/.m2
          key: ${{ runner.os }}-m2-${{ hashFiles('**/pom.xml') }}
          restore-keys: ${{ runner.os }}-m2

      - name: Run Tests
        run: mvn -B test
```

<div align="center">
  <p>Results of the test step.</p>
  <img src="images/b9d58f54f6b44aed87744548786e48c5.png">
</div>

We can run any job in the workflow manually and add different trigger options for the Pipeline

```yml
on:
  #Manually trigger runs
  workflow_dispatch:
  #Trigger workflow on push from main
  push:
    branches: [ main ]
  pull_request:
    types: [ opened, synchronize, reopened ]
```

<div align="center">
  <img src="images/13d001ac8f8a422bb9ed372668992d90.png">
  <p>Workflow Dispatch active</p>
</div>

## 2.2 Quality Gate with SonarQube
### Login to sonarcloud with GitHub
<div align="left">
  <img src="images/d8d04f92509845c28db7b60bcab4bdb3.png" width="350px">
</div>

next, add repo to the sonar install
<div align="left">
  <img src="images/7582a4a0c6864c9ca6da5ba5bce792b2.png" width="350px">
</div>

set up organization name in sonar
<div align="left">
  <img src="images/074ff759fe6b41e4bb42d71eaa315f80.png" width="500px">
    <p>This key is the unique identifier of your organization. You will have to include it as a parameter when configuring your analysis. It could be the name of your company or your team.
</p>
</div>

Select Free Plan
<div align="left">
  <img src="images/0e4d64e1e514464b8aac213cb980a789.png" width="500px">
</div>

Select Project to Analyze
<div align="left">
  <img src="images/dc2f13633b97428bbf828b55e3cba42d.png" width="500px">
</div>

Now we select the analysis method (GitHub Actions)
<div align="left">
  <img src="images/6715c53aa0534410b698fd0fff5e4019.png" width="500px">
</div>

And we set New Code Analysis to Previous Version
<div align="left">
  <img src="images/4f6acf5f21274ddc8e7066d13637f801.png" width="500px">
</div>

Then, we create a Secret in GH using this values
<div align="left">
  <img src="images/4ce3f58006a3428dbcba8477c6d7e553.png" width="500px">
</div>

<div align="left">
  <img src="images/19694b159ec448a9a463a50def550ce9.png" width="500px">
</div>

<div align="left">
  <img src="images/0a35d032bb59488c8aa78ea49acf7466.png" width="500px">
</div>

Now, we select the Build Configuration for our project
<div align="left">
  <img src="images/271655eae412458eacf32e6e981442cc.png" width="500px">
</div>

DO NOT FORGET TO DISABLE AUTOMATIC ANALYSIS
<div align="left">
  <img src="images/1892acbb16994665ab205c412186729e.png" width="500px">
</div>

Now we have to setup our pom.yml with the given parameters in the previous example
<div align="left">
  <img src="images/58c4270412d442278f2ed08c994f5ae7.png" width="500px">
</div>

Everything ok, but code coverage still missing...
<div align="left">
  <img src="images/549c666fb5c04ec0813dccf8c64ba8aa.png" width="500px">
</div>

Adding JavaCodeCoverage dependencies and plugin to pom.xml
![alt text](images/7e9dfa2b295f4605b6644f714618e988.png?raw=true)

![alt text](images/881dc431a3f4497581116ed09c6c535f.png?raw=true)


Now we set up Build Stage.
```yml
build:
    #Make Sonar run Build job parallel to Tests job and build only when these steps do not fail
    needs: [ tests, sonar ]
    runs-on: ubuntu-latest
    steps:
    - uses: actions/checkout@v1
    - name: Set up JDK 11
      uses: actions/setup-java@v1
      with:
        java-version: '11'

#   Adding Cache Action to build stage
    - name: Cache Maven packages
      uses: actions/cache@v1
      with:
        path: ~/.m2
        key: ${{ runner.os }}-m2-${{ hashFiles('**/pom.xml') }}
        restore-keys: ${{ runner.os }}-m2

    - name: Build with Maven
      run: mvn -B package -DskipTests --file pom.xml

    #Upload artifacts workflow allowing to share data between jobs and store data once a workflow is complete.
    - name: Upload JAR
      uses: actions/upload-artifact@v2
      with:
        name: artifact
        path: ${{ github.workspace }}
```

Here we can already see Tests and Sonar jobs running parallel, and build Job depending on boths previous jobs to succeed
![alt text](images/ee345cc2cfd14f7ebeb5862b25de590a.png?raw=true)

And we have Code Coverage!
![alt text](images/9c1a81626b8547509fee021a8239c09d.png?raw=true)

Adding Sonar Quality Gate to Sonar job
```yml
  sonar:
    continue-on-error: true
    name: SonarCloud Analysis (allow failure)
    needs: [ gitleaks, snyk ]
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v1
      - name: Set up JDK 11
        uses: actions/setup-java@v1
        with:
          java-version: '11'
#          fetch-depth: 0
      - name: Cache SonarCloud packages
        uses: actions/cache@v1
        with:
          path: ~/.sonar/cache
          key: ${{ runner.os }}-sonar
          restore-keys: ${{ runner.os }}-sonar
      - name: Cache Maven packages
        uses: actions/cache@v1
        with:
          path: ~/.m2
          key: ${{ runner.os }}-m2-${{ hashFiles('**/pom.xml') }}
          restore-keys: ${{ runner.os }}-m2
      #Analyze project with SonarCloud
      - name: Clone Repo
        uses: actions/checkout@v2
      - name: Analyze with SonarCloud
        run: mvn -B verify sonar:sonar -Dsonar.qualitygate.wait=true
        env:
          GITHUB_TOKEN: ${{ secrets.GITHUB_TOKEN }}
          SONAR_TOKEN: ${{ secrets.SONAR_TOKEN }}
```
![alt text](images/f1a0877b400c430d9f0fef6f41f420f1.png?raw=true)

Make sure to set up timeout-minutes property in your step, to avoid wasting action minutes per month (see above example).
tutorial used:
https://github.com/marketplace/actions/sonarqube-quality-gate-check

Now we need to set up Quality Gate scope
![alt text](images/61b63bdc7db44497bd68fbe968eec594.png?raw=true)

Create a new QG from Organization's Settings:
On new and overall Code
![alt text](images/76c5e03eb7364eaaac4e7e1656990e88.png?raw=true)

![alt text](images/6d49d58c9f40473184e547f937cccb9a.png?raw=true)

for now we set these 2 to test that QG works. Then we can set up further conditions
REMEMBER TO SET THE NEW QG AS DEFAULT
![alt text](images/1c268a0f6da84954b60e915c1c253bdc.png?raw=true)

Sonar job should Fail due to QG settings
![alt text](images/Screenshot_20220509_215201.png?raw=true)
![alt text](images/Screenshot_20220509_215320.png?raw=true)
In this case we set up `continue-on-error: true` param to be able to build and continue with the tasks.

## 2.3 Storing Artifacts with JFrog Artifactory

Final Build Job

```yml
  build:
    name: Build
    #Make Sonar run Build job parallel to Tests job and build only when these steps do not fail
    needs: [ sonar, tests ]
    runs-on: ubuntu-latest
    steps:
    - uses: actions/checkout@v2
    - name: Set up JDK 11
      uses: actions/setup-java@v1
      with:
        java-version: '11'

#   Adding Cache Action to build stage
    - name: Cache Maven packages
      uses: actions/cache@v1
      with:
        path: ~/.m2
        key: ${{ runner.os }}-m2-${{ hashFiles('**/pom.xml') }}
        restore-keys: ${{ runner.os }}-m2

    - name: Set Up Custom Artifactory Instance
      uses: actions/setup-java@v1
      with:
        java-version: '11'
        server-id: artifactory
        server-username: ARTIFACTORY_USERNAME_REF
        server-password: ARTIFACTORY_TOKEN_REF

    - name: Build with Maven and Upload to Artifactory.
      run: mvn -B package -DskipTests --file pom.xml
      env:
        ARTIFACTORY_TOKEN_REF: ${{ secrets.ARTIFACTORY_TOKEN }}
        ARTIFACTORY_USERNAME_REF: ${{ secrets.ARTIFACTORY_USERNAME }}

#    Upload artifacts workflow allowing to share data between jobs and store data once a workflow is complete.
    - name: Upload JAR
      uses: actions/upload-artifact@v2
      with:
        name: build-artifact
        path: ${{ github.workspace }}
```
POM.xml configuration
https://github.com/kawak1320/ci-final-project/blob/main/pom.xml

Adding SCM config to POM.xml
![alt text](images/909f97058fbf41a89a292368b77a0550.png?raw=true)

JFrog Artifactory Repos
![alt text](images/Screenshot_20220509_222344.png)
Here i've been having issues to upload correctly the artifacts to artifactory. Still couldn't find the proper setting.
I will do further research on Maven and JFrog settings.

## 2.4 Integrating DevSecOps
### Gitleaks
```yml
  gitleaks:
    name: gitleaks
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v2
        with:
          fetch-depth: '0'
      - name: gitleaks-action
        uses: zricethezav/gitleaks-action@master
```
![alt text](images/Screenshot_20220509_223350.png?raw=true)

### Snyk
```yml
  snyk:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@master
      - name: Run Snyk to check for vulnerabilities
        uses: snyk/actions/maven@master
        continue-on-error: true # To make sure that SARIF upload gets called
        env:
          SNYK_TOKEN: ${{ secrets.SNYK_TOKEN }}
        with:
          args: --sarif-file-output=snyk.sarif
```
![alt text](images/Screenshot_20220509_223611.png?raw=true)

## 2.5 Additional tasks
- Leverage the GitLab/GitHub cache feature in the pipeline
<p>This step was applied in every Job.</p>

```yml
#   Adding Cache Action to build stage
    - name: Cache Maven packages
      uses: actions/cache@v1
      with:
        path: ~/.m2
        key: ${{ runner.os }}-m2-${{ hashFiles('**/pom.xml') }}
        restore-keys: ${{ runner.os }}-m2
```
- Make some stages run in parallel – for example have unit tests run in parallel with sonar, not sequentially
![alt text](images/Screenshot_20220509_220650.png?raw=true)

- Find and fix a vulnerability found by security tools – if you do this, should be shown what was fixed as a merge request.

- Create a docker image of your application and store in the GitLab Docker Registry or Artifactory or AWS ECR as part of the pipeline