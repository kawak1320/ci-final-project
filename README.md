<div align="center">
  <h1>ci-final-project</h1>
</div>

# Added Project to the Repo


# Select Java with Maven Action


# Adding Unit tests to the workflow

```
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
  <img src="images/b9d58f54f6b44aed87744548786e48c5.png">
  <p>Results of the test step.</p>
</div>

We can run any job in the workflow manually
```
on:
  #Manually trigger runs
  workflow_dispatch:
  #Trigger workflow on push from main
  push:
    branches: [ main ]
  pull_request:
    types: [ opened, synchronize, reopened ]
```
and added different trigger options for the Pipeline

<div align="center">
  <img src="images/13d001ac8f8a422bb9ed372668992d90.png">
  <p>Workflow Dispatch active</p>
</div>

Login to sonarcloud with GitHub
![alt text](images/d8d04f92509845c28db7b60bcab4bdb3.png?raw=true)

next, add repo to the sonar install
![alt text](images/7582a4a0c6864c9ca6da5ba5bce792b2.png?raw=true)

set up organization name in sonar
![alt text](images/074ff759fe6b41e4bb42d71eaa315f80.png?raw=true)

This key is the unique identifier of your organization. You will have to include it as a parameter when configuring your analysis. It could be the name of your company or your team.

Select Free Plan
![alt text](images/0e4d64e1e514464b8aac213cb980a789.png?raw=true)

Select Project to Analyze
![alt text](images/dc2f13633b97428bbf828b55e3cba42d.png?raw=true)

Now we select the analysis method (GitHub Actions)
![alt text](images/6715c53aa0534410b698fd0fff5e4019.png?raw=true)

And we set New Code Analysis to Previous Version
![alt text](images/4f6acf5f21274ddc8e7066d13637f801.png?raw=true)

Then, we create a Secret in GH
![alt text](images/4ce3f58006a3428dbcba8477c6d7e553.png?raw=true)

![alt text](images/19694b159ec448a9a463a50def550ce9.png?raw=true)

![alt text](images/0a35d032bb59488c8aa78ea49acf7466.png?raw=true)

Now, we need to update our config files in the repo
![alt text](images/271655eae412458eacf32e6e981442cc.png?raw=true)

DO NOT FORGET TO DISABLE THIS
![alt text](images/1892acbb16994665ab205c412186729e.png?raw=true)

Now we can do our magic in the IDE
![alt text](images/58c4270412d442278f2ed08c994f5ae7.png?raw=true)

![alt text](images/0842009cf83448a6a5022db9b271daaf.png?raw=true)

Everything ok, but we have no coverage...
![alt text](images/549c666fb5c04ec0813dccf8c64ba8aa.png?raw=true)

Adding JavaCodeCoverage dependencies and plugin to pom.xml
![alt text](images/7e9dfa2b295f4605b6644f714618e988.png?raw=true)

![alt text](images/881dc431a3f4497581116ed09c6c535f.png?raw=true)

Here we can already see Tests and Sonar jobs running parallel, and build Job depending on boths previous jobs to succeed
![alt text](images/ee345cc2cfd14f7ebeb5862b25de590a.png?raw=true)

And we have Code Coverage!
![alt text](images/9c1a81626b8547509fee021a8239c09d.png?raw=true)


Now we set up Build Stage to push the JAR file to the workflow
We will use this JAR file in the deploy job
  ```
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
    - name: Upload JAR
    #Upload artifacts workflow allowing to share data between jobs and store data once a workflow is complete.
      uses: actions/upload-artifact@v2
      with:
        name: artifact
        path: ${{ github.workspace }}
```


Adding Sonar Quality Gate to Sonar job
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

Sonar job should Fail due to QG settings:
Adding SCM config to POM
![alt text](images/909f97058fbf41a89a292368b77a0550.png?raw=true)



---------------------------------FALTA QUE FUNCIONE QUALITY GATE --------------------------------

JFrog Artifactory
![alt text](images/e1800ca03332459d973e9b482f0e2272.png)
