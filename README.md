# ci-final-project

# ci-final-project

Added Project to the Repo
Select Java with Maven Action


![alt text](https://github.com/kawak1320/ci-final-project/blob/main/images/074ff759fe6b41e4bb42d71eaa315f80.png?raw=true)

Adding Unit tests to the workflow
![7af7ec43f22e550475268d47215320b6.png](:/fd1da4dfc2394bb0ae8e96caf7b7fe1c)
![alt text](https://github.com/kawak1320/ci-final-project/blob/main/images/fd1da4dfc2394bb0ae8e96caf7b7fe1c.png?raw=true)


Result of the test step
![4d4a1b8e02070214ec83175c1a7d2406.png](:/b9d58f54f6b44aed87744548786e48c5)
![alt text](https://github.com/kawak1320/ci-final-project/blob/main/images/b9d58f54f6b44aed87744548786e48c5.png?raw=true)


We can run any job in the workflow mannually
![10c529a0e18fc3cdfb26657f2fd567f8.png](:/07fae8b0da754bfdbbe61d1629f3c66b)
![alt text](https://github.com/kawak1320/ci-final-project/blob/main/images/07fae8b0da754bfdbbe61d1629f3c66b.png?raw=true)

![0295c276705efcabe34997c7b2c93aa5.png](:/13d001ac8f8a422bb9ed372668992d90)
![alt text](https://github.com/kawak1320/ci-final-project/blob/main/images/13d001ac8f8a422bb9ed372668992d90.png?raw=true)


Login to sonarcloud with GitHub
![69d8d4380e9063455005d6e0109628ec.png](:/d8d04f92509845c28db7b60bcab4bdb3)
![alt text](https://github.com/kawak1320/ci-final-project/blob/main/images/d8d04f92509845c28db7b60bcab4bdb3.png?raw=true)


next, add repo to the sonar install
![79d562b218cc2bb753c451a6288da189.png](:/7582a4a0c6864c9ca6da5ba5bce792b2)
![alt text](https://github.com/kawak1320/ci-final-project/blob/main/images/7582a4a0c6864c9ca6da5ba5bce792b2.png?raw=true)


set up organization name in sonar
![c7b4a44349a6b947b60b3950fc80cef0.png](:/074ff759fe6b41e4bb42d71eaa315f80)
![alt text](https://github.com/kawak1320/ci-final-project/blob/main/images/074ff759fe6b41e4bb42d71eaa315f80.png?raw=true)


This key is the unique identifier of your organization. You will have to include it as a parameter when configuring your analysis. It could be the name of your company or your team.


Select Free Plan
![b5bd563d1390a1e67c5a3dc2844e199d.png](:/0e4d64e1e514464b8aac213cb980a789)
![alt text](https://github.com/kawak1320/ci-final-project/blob/main/images/0e4d64e1e514464b8aac213cb980a789.png?raw=true)


Select Project to Analyze
![3333ecb3117a9160d58b476cc248a67e.png](:/dc2f13633b97428bbf828b55e3cba42d)
![alt text](https://github.com/kawak1320/ci-final-project/blob/main/images/dc2f13633b97428bbf828b55e3cba42d.png?raw=true)


Now we select the analysis method (GitHub Actions)
![29efd7a5ce6cb144b79698f39f430c36.png](:/6715c53aa0534410b698fd0fff5e4019)
![alt text](https://github.com/kawak1320/ci-final-project/blob/main/images/6715c53aa0534410b698fd0fff5e4019.png?raw=true)


And we set New Code Analysis to Previous Version
![3b1470597a21da98f9a4128572a66fcd.png](:/4f6acf5f21274ddc8e7066d13637f801)
![alt text](https://github.com/kawak1320/ci-final-project/blob/main/images/4f6acf5f21274ddc8e7066d13637f801.png?raw=true)


Then, we create a Secret in GH
![e20a30956274a35bead009377dd4a955.png](:/4ce3f58006a3428dbcba8477c6d7e553)
![alt text](https://github.com/kawak1320/ci-final-project/blob/main/images/4ce3f58006a3428dbcba8477c6d7e553.png?raw=true)


![cc4dcebf72bc9ea29970fc396c13b51b.png](:/19694b159ec448a9a463a50def550ce9)
![alt text](https://github.com/kawak1320/ci-final-project/blob/main/images/19694b159ec448a9a463a50def550ce9.png?raw=true)


![4ed3687527a03bf141db85ce97384526.png](:/0a35d032bb59488c8aa78ea49acf7466)
![alt text](https://github.com/kawak1320/ci-final-project/blob/main/images/0a35d032bb59488c8aa78ea49acf7466.png?raw=true)


Now, we need to update our config files in the repo
![a7b04483c3527aac0f33cf65fda505e6.png](:/271655eae412458eacf32e6e981442cc)
![alt text](https://github.com/kawak1320/ci-final-project/blob/main/images/271655eae412458eacf32e6e981442cc.png?raw=true)


DO NOT FORGET TO DISABLE THIS
![bac774f06c4a06a7e6a1370eadf30998.png](:/1892acbb16994665ab205c412186729e)
![alt text](https://github.com/kawak1320/ci-final-project/blob/main/images/1892acbb16994665ab205c412186729e.png?raw=true)

Now we can do our magic in the IDE
![0ce1a00e026b4464ebae4c7b22341901.png](:/58c4270412d442278f2ed08c994f5ae7)
![alt text](https://github.com/kawak1320/ci-final-project/blob/main/images/58c4270412d442278f2ed08c994f5ae7.png?raw=true)


![78d88f619eb224f2a0a5acbc2cd78f27.png](:/0842009cf83448a6a5022db9b271daaf)
![alt text](https://github.com/kawak1320/ci-final-project/blob/main/images/0842009cf83448a6a5022db9b271daaf.png?raw=true)


Everything ok, but we have no coverage...
![ee122ca9654195b1178bf9cf589f2b24.png](:/549c666fb5c04ec0813dccf8c64ba8aa)
![alt text](https://github.com/kawak1320/ci-final-project/blob/main/images/549c666fb5c04ec0813dccf8c64ba8aa.png?raw=true)


Adding JavaCodeCoverage dependencies and plugin to pom.xml
![afa58499e87d61444eef989073d09dc4.png](:/7e9dfa2b295f4605b6644f714618e988)
![alt text](https://github.com/kawak1320/ci-final-project/blob/main/images/7e9dfa2b295f4605b6644f714618e988.png?raw=true)


![d7bfb8af651141e32d4f4d6a28af8734.png](:/881dc431a3f4497581116ed09c6c535f)
![alt text](https://github.com/kawak1320/ci-final-project/blob/main/images/881dc431a3f4497581116ed09c6c535f.png?raw=true)


Here we can already see Tests and Sonar jobs running parallel, and build Job depending on boths previous jobs to succeed
![685d84dda1a6503bf427f25104947c70.png](:/ee345cc2cfd14f7ebeb5862b25de590a)
![alt text](https://github.com/kawak1320/ci-final-project/blob/main/images/ee345cc2cfd14f7ebeb5862b25de590a.png?raw=true)


And we have Code Coverage!
![9c2b23f5433972051dd60c1357816e2e.png](:/9c1a81626b8547509fee021a8239c09d)
![alt text](https://github.com/kawak1320/ci-final-project/blob/main/images/9c1a81626b8547509fee021a8239c09d.png?raw=true)


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
![36446ccb78362a166ef258a086ba06e3.png](:/f1a0877b400c430d9f0fef6f41f420f1)
![alt text](https://github.com/kawak1320/ci-final-project/blob/main/images/f1a0877b400c430d9f0fef6f41f420f1.png?raw=true)

Make sure to set up timeout-minutes property in your step, to avoid wasting action minutes per month (see above example).
tutorial used:
https://github.com/marketplace/actions/sonarqube-quality-gate-check

Now we need to set up Quality Gate scope
![1f381228c3c9374d7361958ddecc916f.png](:/61b63bdc7db44497bd68fbe968eec594)
![alt text](https://github.com/kawak1320/ci-final-project/blob/main/images/61b63bdc7db44497bd68fbe968eec594.png?raw=true)


Create a new QG from Organization's Settings:
On new and overall Code
![9ba6c484fa37295919b4ec9dde9ae21b.png](:/76c5e03eb7364eaaac4e7e1656990e88)
![alt text](https://github.com/kawak1320/ci-final-project/blob/main/images/76c5e03eb7364eaaac4e7e1656990e88.png?raw=true)


![a4c7636d6071e8c5c4dffa5db4ee9528.png](:/6d49d58c9f40473184e547f937cccb9a)
![alt text](https://github.com/kawak1320/ci-final-project/blob/main/images/6d49d58c9f40473184e547f937cccb9a.png?raw=true)


for now we set these 2 to test that QG works. Then we can set up further conditions
REMEMBER TO SET THE NEW QG AS DEFAULT
![fe2fa3e02fd5ce0a5113d9483244fa85.png](:/1c268a0f6da84954b60e915c1c253bdc)
![alt text](https://github.com/kawak1320/ci-final-project/blob/main/images/1c268a0f6da84954b60e915c1c253bdc.png?raw=true)


Sonar job should Fail due to QG settings:
Adding SCM config to POM
![07e4272512466156bf6427636fb551c5.png](:/909f97058fbf41a89a292368b77a0550)
![alt text](https://github.com/kawak1320/ci-final-project/blob/main/images/909f97058fbf41a89a292368b77a0550.png?raw=true)



---------------------------------FALTA QUE FUNCIONE QUALITY GATE --------------------------------

JFrog Artifactory
![25bbb2b835081145b07e7e886a36454e.png](:/e1800ca03332459d973e9b482f0e2272)
![alt text](https://github.com/kawak1320/ci-final-project/blob/main/images/e1800ca03332459d973e9b482f0e2272.png =250x)
