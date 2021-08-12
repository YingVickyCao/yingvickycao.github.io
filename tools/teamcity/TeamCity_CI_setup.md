# TeamCity setups

- https://github.com/YingVickyCao/EnableCodeCoverage

# 1 TeanCity CI about versions

| Item                            | Verson                               |
| ------------------------------- | ------------------------------------ |
| Projet Gradle                   | Gradle 4.5                           |
| Projet Gradle plugin            | com.android.tools.build:gradle:3.0.1 |
| Projet Jacoco plugin            | org.jacoco:org.jacoco.core:0.8.4     |
| Projet                          | JDK 1.8                              |
| TeamCity - Build Steps : Gradle | JDK 1.8 x64                          |

- SonarQube 6.7.7

| Item                                      | Verson                                             |
| ----------------------------------------- | -------------------------------------------------- |
| TeamCity - Build Steps : SonarQube Runner | JDK 1.8 x64                                        |
| sonarqube-6.7.7                           | JDK 1.8 <br/>Java <br/> `sonar.jacoco.reportPaths` |

- SonarQube 7.7

| Item                                      | JDK Verson                                          |
| ----------------------------------------- | --------------------------------------------------- |
| TeamCity - Build Steps : SonarQube Runner | JDK 1.8 x64                                         |
| sonarqube-7.7                             | JDK 1.8 <br/> Java <br/> `sonar.jacoco.reportPaths` |

- SonarQube 7.9

| Item                                      | JDK Verson                                                           |
| ----------------------------------------- | -------------------------------------------------------------------- |
| TeamCity - Build Steps : SonarQube Runner | JDK 11 x64                                                           |
| sonarqube-7.9                             | JDK 11 <br/> Java+Kotlin <br/> sonar.coverage.jacoco.xmlReportPaths` |

# 2 Step

https://github.com/YingVickyCao/EnableCodeCoverage  
https://gitee.com/YingVickyCao/EnableCodeCoverage

```
gradle clean :app:testDebug :androidLib:testDebug :javalib:test

/Users/hades/Library/TeamCity/bin/runAll.sh start
/Users/hades/Library/TeamCity/bin/runAll.sh stop

/Users/hades/Library/sonarqube/bin/macosx-universal-64/sonar.sh start
/Users/hades/Library/sonarqube/bin/macosx-universal-64/sonar.sh stop
```

## 配置 when test on 2021/8/12, TeamCity 2021.1.2 (build 92869)

```
android studio 4.2.2,need JDK 1.8
androidx
gradle 6.9
android gradle plugin 4.0.2
compole SDK 30
target 30
minSDK 23
build tools:30.0.3

Java 1.8
SonarQube server - Community EditionVersion 7.9.6 (build 41879), need JDK 11
TeamCity 2021.1.2 (build 92869),need JDK 1.8
```

## Step 1: Prepare Android Code

- [Jacoco](/doc/tools/jacoco/Jacoco.md)

## Step 2: SonarQube

- [SonarQube](/doc/tools/sonarqube/SonarQube_Config.md)

## Step 3: TeamCity

- [TeamCity - MarkDown](/doc/tools/teamcity/TeamCity.md)

- [TeamCity - html](https://yingvickycao.github.io/doc/tools/teamcity/TeamCity.html)

## Step4 TeamCity Run

点击 run，开始手动构建该项目

# 3 Conclusion

- Read the official website information
- Local Jacoco coverage report exported success, then try config TeamCity
- Trial & Error 不断试错
- Note version difference.  
  e.g.  
  gradle plugin effect on Jacoco coverage report exported.  
  JDK  
  Sonarqube
- Refer to already scucessful setup
