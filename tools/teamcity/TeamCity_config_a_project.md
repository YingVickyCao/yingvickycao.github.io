# TeamCity Config a project

# 1 General Settings

![TeamCity_GeneralSettings](https://yingvickycao.github.io/img/TeamCity_GeneralSettings.jpg)

- Build number format:  
  `%VersionNumber%`
- Artifact paths:  
  `app/build/outputs => app/build/outputs`

# 2 Version Control Settings

![TeamCity_Version_Control_Settings.jpg](https://yingvickycao.github.io/img/TeamCity_Version_Control_Settings.jpg)

- Branch Filter
  `+:*`

## VCS Roots - Edit VCS Root

![TeamCity_Version_Control_Settings_Edit_VCS_Root_1.jpg](https://yingvickycao.github.io/img/TeamCity_Version_Control_Settings_Edit_VCS_Root_1.jpg)

![TeamCity_Version_Control_Settings_Edit_VCS_Root_2.jpg](https://yingvickycao.github.io/img/TeamCity_Version_Control_Settings_Edit_VCS_Root_2.jpg)

![TeamCity_Version_Control_Settings_Edit_VCS_Root.jpg](https://yingvickycao.github.io/img/TeamCity_Version_Control_Settings_Edit_VCS_Root_3.jpg)

- Fetch URL:  
  `https://github.com/YingVickyCao/EnableCodeCoverage.git`

- Default branch:  
  `%defaultBranchVal%`
- Branch specification:  
  `%addtionalBrtanchSpec%`
- Click Test Connnection

# 3 Build Steps

![TeamCity_Build_Steps.jpg](https://yingvickycao.github.io/img/TeamCity_Build_Steps.jpg)

# 4 Build Step (1 of 4): Android SDK update

![img/TeamCity_Build_Steps_4_1_android_sdk_update.jpg.jpg](https://yingvickycao.github.io/img/TeamCity_Build_Steps_4_1_android_sdk_update.jpg)

```
# android update sdk --use-sdk-wrapper --no-ui --all --force  --proxy-host mirrors.neusoft.edu.cn --proxy-port 80
echo 'y'|/%env.ANDROID_HOME%/tools/android update sdk --use-sdk-wrapper --no-ui --all --force  --proxy-host %env.proxy_host% --proxy-port %env.proxy_port%
pwd
npm config list

```

```
[19:42:29][Step 1/4] The "android" command is deprecated.
[19:42:29][Step 1/4] For manual SDK, AVD, and project management, please use Android Studio.
[19:42:29][Step 1/4] For command-line tools, use tools/bin/sdkmanager and tools/bin/avdmanager
The "android" command is deprecated. For command-line tools, use tools/bin/sdkmanager and tools/bin/avdmanager
```

```
# sdk/tools/bin/sdkmanager --update --proxy_host=mirrors.neusoft.edu.cn --proxy_port=80 --proxy=http
echo 'y'|/%env.ANDROID_HOME%/tools/bin/sdkmanager --update --proxy_host=%env.proxy_host% --proxy_port=%env.proxy_port% --proxy=http
pwd
npm config list
```

# 5 Build Step- (2 of 4): Gradle

![TeamCity_Build_Steps_Step_Gradle.jpg](https://yingvickycao.github.io/img/TeamCity_Build_Steps_Step_Gradle.jpg)

- `%gradleTasks%`

- build.gradle
- JDK

# Build Step (4 of 4): SonarQube Analysis

![TeamCity_Build_Steps_Step_Sonar_QubeAnalysis.jpg](https://yingvickycao.github.io/img/TeamCity_Build_Steps_Step_Sonar_QubeAnalysis.jpg)

- Runner Type：SonarQube Runner  
  Teamcity 如果默认没有 SonarQube Runner， 需要下载。

![Teamcity_2020.1.3_install_SonarQube_Analysis_plugin_1](https://yingvickycao.github.io/img/tools/teamcity/Teamcity_2020.1.3_install_SonarQube_Analysis_plugin_1.jpg)

![Teamcity_2020.1.3_install_SonarQube_Analysis_plugin_2](https://yingvickycao.github.io/img/tools/teamcity/Teamcity_2020.1.3_install_SonarQube_Analysis_plugin_2.jpg)

![Teamcity_2020.1.3_install_SonarQube_Analysis_plugin_3](https://yingvickycao.github.io/img/tools/teamcity/Teamcity_2020.1.3_install_SonarQube_Analysis_plugin_3.jpg)

![Teamcity_2020.1.3_install_SonarQube_Analysis_plugin_4](https://yingvickycao.github.io/img/tools/teamcity/Teamcity_2020.1.3_install_SonarQube_Analysis_plugin_4.jpg)

![Teamcity_2020.1.3_install_SonarQube_Analysis_plugin_5](https://yingvickycao.github.io/img/tools/teamcity/Teamcity_2020.1.3_install_SonarQube_Analysis_plugin_5.jpg)

Failed to download plugin from URL https://plugins.jetbrains.com/plugin/download?updateId=118904&build=92869&mode=professional&uuid=64f6f93f-7ef1-465b-a5dc-59c18fdf9696: Unable to connect to the plugins repository https://plugins.jetbrains.com. Check the network settings and try again later.

下载失败了，手动下载 URL，然后使用“Upload plugin zip” 来安装。

![Teamcity_2020.1.3_install_SonarQube_Analysis_plugin_6](https://yingvickycao.github.io/img/tools/teamcity/Teamcity_2020.1.3_install_SonarQube_Analysis_plugin_6.jpg)

- Project name:  
  `%env.sonarProjectName%`
- Project key:  
  `%env.sonarProjectKey%`
- Project version:  
  `%build.number%`

- Tests location

```
%teamcity.build.checkoutDir%/app/src/test/java,%teamcity.build.checkoutDir%/javaLib/src/test/java,%teamcity.build.checkoutDir%/androidLib/src/test/java
```

等价于 `sonar.tests`. 使用`Additional parameters`,而不是此处

ERROR:

```
[16:45:14]Caused by: The folder 'src/main/java' does not exist for 'EnableCodeCoverage' (base directory = /Users/account/Library/TeamCity/buildAgent/work/b5c3f37037cf919e)
```

```
/**/src/main/test
**/src/main/test
%teamcity.build.checkoutDir%/**/src/test/java
%teamcity.build.checkoutDir%/**/src/main/test
%teamcity.build.checkoutDir%/javaLib/src/test,%teamcity.build.checkoutDir%/androidLib/src/test
%teamcity.build.checkoutDir%/app/src/main/test,%teamcity.build.checkoutDir%/javaLib/src/main/test,%teamcity.build.checkoutDir%/androidLib/src/main/test
```

- Binaries location
- JDK
- Sources location

```
%teamcity.build.checkoutDir%/app/src/main/java,%teamcity.build.checkoutDir%/javaLib/src/main/java,%teamcity.build.checkoutDir%/androidLib/src/main/java
```

等价于 `sonar.sources`. 使用`Additional parameters`,而不是此处

ERROR:

```
src/main/java
**/src/main/java
%teamcity.build.checkoutDir%/**/src/main/java
%teamcity.build.checkoutDir%/app/src/test/java
```

## Additional parameters:

| Symbol | Meaning                   |
| ------ | ------------------------- |
| -D     | 配置属性                  |
| ?      | a single character        |
| \*     | any number of characters  |
| \* \*  | any number of directories |

SonarQube 7.7:

```groovy
-Dsonar.jacoco.reportPaths=app/build/jacoco/jacocoTest.exec,androidLib/build/jacoco/jacocoTest.exec,javaLib/build/jacoco/jacocoTest.exec
-Dsonar.java.binaries=app/build/intermediates/classes/debug/,javaLib/build/classes/java/,androidLib/build/intermediates/classes/debug/
-Dsonar.tests=app/src/test/java,javaLib/src/test/java,androidLib/src/test/java
-Dsonar.sources=app/src/main/java,javaLib/src/main/java,androidLib/src/main/java
-Dsonar.exclusions=**/*Activity.java,**/*.kt,/**/gen/**/*,/**/build/**/*,**/R.class,**/R$*.class,**/BuildConfig.*,**/Manifest*.*,**/Lambda$*.class,**/Lambda.class,**/*Lambda.class,**/*Lambda*.class,**/*Test*.*,**/*ViewBinder*.*,**/*ViewInjector*.*,android/**/*.*,**/*Fragment.*
-Dsonar.login=%env.sonarLogin%
-Dsonar.password=%env.sonarPassword%
-Dsonar.scm.disable=false
-Dsonar.scm.url=http://github.com/YingVickyCao/AUtils/tree/master
-Dsonar.scm.connection=scm:git:ssh://github.com:YingVickyCao/AUtils.git
-Dsonar.scm.developerConnection=scm:git:ssh://github.com:YingVickyCao/AUtils.git
-Dsonar.scm.forceReloadAll=true
-Dsonar.sourceEncoding=UTF-8
-X
```

SonarQube 7.9:

```groovy
-Dsonar.coverage.jacoco.xmlReportPaths=app/build/reports/jacoco/jacocoTestReport/jacocoTestReport.xml,javaLib/build/reports/jacoco/test/jacocoTestReport.xml,androidLib/build/reports/jacoco/jacocoTestReport/jacocoTestReport.xml
-Dsonar.java.binaries=app/build/intermediates/classes/debug/,app/build/tmp/kotlin-classes/debug/,javaLib/build/classes/java/,androidLib/build/intermediates/classes/debug/,androidLib/build/tmp/kotlin-classes/debug
-Dsonar.tests=app/src/test/java,javaLib/src/test/java,androidLib/src/test/java
-Dsonar.sources=app/src/main/java,javaLib/src/main/java,androidLib/src/main/java
-Dsonar.exclusions=**/*Activity.java,**/*Activity.kt,/**/gen/**/*,/**/build/**/*,**/R.class,**/R$*.class,**/BuildConfig.*,**/Manifest*.*,**/Lambda$*.class,**/Lambda.class,**/*Lambda.class,**/*Lambda*.class,**/*Test*.*,**/*ViewBinder*.*,**/*ViewInjector*.*,android/**/*.*,**/*Fragment.*
-Dsonar.login=%env.sonarLogin%
-Dsonar.password=%env.sonarPassword%
-Dsonar.scm.disable=false
-Dsonar.scm.url=http://github.com/YingVickyCao/AUtils/tree/master
-Dsonar.scm.connection=scm:git:ssh://github.com:YingVickyCao/AUtils.git
-Dsonar.scm.developerConnection=scm:git:ssh://github.com:YingVickyCao/AUtils.git
-Dsonar.scm.forceReloadAll=true
-Dsonar.sourceEncoding=UTF-8
-X
```

SonarQube 6.7.7:

```groovy
-Dsonar.jacoco.reportPaths=app/build/jacoco/jacocoTest.exec,androidLib/build/jacoco/jacocoTest.exec,javaLib/build/jacoco/jacocoTest.exec
-Dsonar.java.binaries=app/build/intermediates/classes/debug/,javaLib/build/classes/java/,androidLib/build/intermediates/classes/debug/
-Dsonar.tests=app/src/test/java,app2/src/test/java,javaLib/src/test/java,androidLib/src/test/java
-Dsonar.sources=app/src/main/java,app2/src/main/java,javaLib/src/main/java,androidLib/src/main/java
-Dsonar.exclusions=**/*Activity.java,**/*.kt,/**/gen/**/*,/**/build/**/*,**/R.class,**/R$*.class,**/BuildConfig.*,**/Manifest*.*,**/Lambda$*.class,**/Lambda.class,**/*Lambda.class,**/*Lambda*.class,**/*Test*.*,**/*ViewBinder*.*,**/*ViewInjector*.*,android/**/*.*,**/*Fragment.*,**/*Module.*,**/*Module4Dagger2.*,**/*Component.*
-Dsonar.login=%env.sonarLogin%
-Dsonar.password=%env.sonarPassword%
-Dsonar.scm.disable=false
-Dsonar.scm.url=http://github.com/YingVickyCao/AUtils/tree/master
-Dsonar.scm.connection=scm:git:ssh://github.com:YingVickyCao/AUtils.git
-Dsonar.scm.developerConnection=scm:git:ssh://github.com:YingVickyCao/AUtils.git
-Dsonar.scm.forceReloadAll=true
-Dsonar.sourceEncoding=UTF-8
-X
```

`-Dsonar.branch.name=%teamcity.build.branch%`:  
Branch analysis is available as part of `Developer Edition`  
https://docs.sonarqube.org/latest/branches/overview/

- JacocoReport - classDirectories - excludes , not work  
  sonar.exclusions, work.

https://docs.sonarqube.org/7.9/project-administration/narrowing-the-focus/

```
**/*Module.*,**/*Module4Dagger2.*
```

![TeamCity_Build_Steps_Step_Sonar_QubeAnalysis_filter_code_coverage_4_dagger_before](https://yingvickycao.github.io/img/TeamCity_Build_Steps_Step_Sonar_QubeAnalysis_filter_code_coverage_4_dagger_before.jpg)

![TeamCity_Build_Steps_Step_Sonar_QubeAnalysis_filter_code_coverage_4_dagger_after](https://yingvickycao.github.io/img/TeamCity_Build_Steps_Step_Sonar_QubeAnalysis_filter_code_coverage_4_dagger_after.jpg)

- `sonar.scm.*` is optional

# 6 Triggers（触发器）

## VCS Trigger：自动构建触发行为。当有代码提交的时候，TeamCity 检查到新版本之后自动构建，

- TeamCity 会根据您设置的时间间隔去检测代码的变化。如果这段时间中有多个 checkin，仅触发一次 build。
- 当一个 build 配置有多个 VCS root 时，并不会为每个 VCS root 的变化触发 build，而是在检测过所有的 VCS root 后才决定是否触发一次 build
- 设置每个 checkin 都触发一次 build  
  Choose `选择"Trigger a build on each check-in"`
- 同一个提交者的多次提交只触发一次 build  
  Choose `"Include seral check-ins in a build if they are from the same committer"`
- 静默期(Quiet Period)  
  当检测到最后一次变更后的一段时间(默认一分钟)内没有发现新的变更才触发一次 build。

## Schedule Trigger：定时构建

![TeamCity_Triggers_Add_New_Triger.jpg](https://yingvickycao.github.io/img/TeamCity_Triggers_Add_New_Triger.jpg)

![TeamCity_Triggers.jpg](https://yingvickycao.github.io/img/TeamCity_Triggers.jpg)

# 7 Parameters

## Configuration Parameters

| Name                  | Value                                                    |
| --------------------- | -------------------------------------------------------- |
| addtionalBrtanchSpec  | See Below                                                |
| defaultBranchVal      | refs/heads/master                                        |
| gradleTasks           | clean :app:testDebug :androidLib:testDebug :javaLib:test |
| sonar.project.version | %VersionNumber%                                          |
| VersionNumber         | 1.0.0.%build.counter% Edit Delete                        |

- addtionalBrtanchSpec

```
+:refs/heads/*
+:refs/(pull-requests/*)/merged
```

## System Properties (system.)

## Environment Variables (env.)

| Name                    | Value                      |
| ----------------------- | -------------------------- |
| env.sonarLogin          | admin                      |
| env.sonarPassword       | admin                      |
| env.sonarProjectKey     | EnableCodeCoverage         |
| env.sonarProjectName    | Android-EnableCodeCoverage |
| env.sonarProjectVersion | %sonar.project.version%    |
| env.proxy_host          | mirrors.neusoft.edu.cn     |
| env.proxy_port          | 80                         |

# FAQ

## ERROR : `Please supply original non-instrumented`

![TeamCity_Build_Steps_Step_Gradle_Code_Coverage_1.jpg](https://yingvickycao.github.io/img/TeamCity_Build_Steps_Step_Gradle_Code_Coverage_2.jpg)

Fix:  
去掉 Coverage 设置  
![TeamCity_Build_Steps_Step_Gradle_Code_Coverage_1.jpg](https://yingvickycao.github.io/img/TeamCity_Build_Steps_Step_Gradle_Code_Coverage_1.jpg)

## ERROR ： `It is not located in module basedir`

```
[[17:32:31]17:32:31.865 WARN: File '/Users/account/Library/TeamCity/buildAgent/work/b5c3f37037cf919e/app/src/main/java/com/github/yingvickycao/enablecodecoverage/A1.java' is ignored. It is not located in module basedir '/Users/account/Library/TeamCity/buildAgent/work/b5c3f37037cf919e/androidLib'.
```

Fix:  
去掉 modules 设置

## ERROR: `An illegal reflective access operation has occurred`

```
WARNING: An illegal reflective access operation has occurred
[16:16:03]WARNING: Illegal reflective access by net.sf.cglib.core.ReflectUtils$1 (file:/Users/account/.sonar/cache/866bb1adbf016ea515620f1aaa15ec53/sonar-javascript-plugin.jar) to method java.lang.ClassLoader.defineClass(java.lang.String,byte[],int,int,java.security.ProtectionDomain)
[16:16:03]WARNING: Please consider reporting this to the maintainers of net.sf.cglib.core.ReflectUtils$1
[16:16:03]WARNING: Use --illegal-access=warn to enable warnings of further illegal reflective access operations
[16:16:03]WARNING: All illegal access operations will be denied in a future release
[16:16:04]16:16:04.019 ERROR: Invalid value for sonar.java.binaries
[16:16:04]16:16:04.045 ERROR: Error during SonarQube Scanner execution
[16:16:04]java.lang.IllegalStateException: No files nor directories matching ' app/build/tmp/kotlin-classes/debug/'
[16:16:04] at org.sonar.java.AbstractJavaClasspath.getFilesFromProperty(AbstractJavaClasspath.java:92)
```

Fix:

```
-Dsonar.java.binaries=app/tmp/kotlin-classes/debug/
=>
-Dsonar.java.binaries=app/build/tmp/kotlin-classes/debug/
```

## ERROR：`Caused by: Not authorized. Please check the properties sonar.login and sonar.password.`

Fix:
sonar.login admin, not tocken  
sonar.password admin

## ERROR: `Invalid value for sonar.java.binaries`

```
16:53:23.035 WARN: SCM provider autodetection failed. Please use "sonar.scm.provider" to define SCM of your project, or disable the SCM Sensor in the project settings.
[16:53:25]16:53:25.166 ERROR: Invalid value for sonar.java.binaries
[16:53:25]16:53:25.288 ERROR: Error during SonarQube Scanner execution
```

Fix:

```
# android Gradle buils system Java Byte Code Location. So does Android Project build.gradle config jacocoTestReport.
# classDirectories 的设置应以项目编译后生成的 class 文件目录为准
# android gradle plugin < 3.2
-Dsonar.java.binaries=app/build/intermediates/classes/debug

# android gradle plugin >= 3.2
-Dsonar.java.binaries=app/build/intermediates/javac/debug/compileDebugJavaWithJavac/classes
```

## ERROR: `Invalid value for sonar.java.binaries`

```
[Step 3/3] 16:08:23.511 ERROR: Invalid value for sonar.java.binaries
[Step 3/3] java.lang.IllegalStateException: No files nor directories matching ' app/tmp/kotlin-classes/debug/'
```

Fix:

```
app/tmp/kotlin-classes/debug/
=>
app/build/tmp/kotlin-classes/debug/
```

## ERROR: `WARN: SCM provider autodetection failed. Please use "sonar.scm.provider" to define SCM of your project, or disable the SCM Sensor in the project settings.`

```
-Dsonar.scm.disable=false
-Dsonar.scm.url=http://github.com/YingVickyCao/AUtils/tree/master
-Dsonar.scm.connection=scm:git:ssh://github.com:YingVickyCao/AUtils.git
-Dsonar.scm.developerConnection=scm:git:ssh://github.com:YingVickyCao/AUtils.git
-Dsonar.scm.forceReloadAll=true
```

## ERROR: TBD, `commit hook is inactive`

https://www.jetbrains.com/help/teamcity/2019.1/configuring-vcs-post-commit-hooks-for-teamcity.html

## ERROR: `Caused by: java.lang.LinkageError: loader 'bootstrap' attempted duplicate class definition for java.lang.$JaCoCo`

```
[21:37:59]:app:mergeDebugResources
[21:37:59][:app:mergeDebugResources] Exception in thread "main" java.lang.reflect.InvocationTargetException
[21:37:59][:javaLib:test] Caused by: java.lang.reflect.InvocationTargetException
[21:37:59][:javaLib:test] Caused by: java.lang.LinkageError: loader 'bootstrap' attempted duplicate class definition for java.lang.$JaCoCo.
[21:37:59][:app:mergeDebugResources] Caused by: java.lang.reflect.InvocationTargetException
[21:37:59][:app:mergeDebugResources] Caused by: java.lang.LinkageError: loader 'bootstrap' attempted duplicate class definition for java.lang.$JaCoCo.

[21:38:00]> Process 'Gradle Test Executor 1' finished with non-zero exit value 134
[21:38:00]Caused by: org.gradle.process.internal.ExecException: Process 'Gradle Test Executor 1' finished with non-zero exit value 134
```

Fix:

- Clear `/Users/account/Library/TeamCity/buildAgent/work/`
- jdk 版本不对应

## ERROR:`Unrecognized option`

```
[Step 3/3] ERROR: Unrecognized option: -sonar.jacoco.reportPaths=app/build/jacoco/jacocoTest.exec,androidLib/build/jacoco/jacocoTest.exec,javaLib/build/jacoco/jacocoTest.exec
-D

```

Fix:

```
-sonar.jacoco.reportPaths
=>
-Dsonar.jacoco.reportPaths
```

## Q : [TeamCity 2021.1.2 (build 92869)] Can not find SonarQube Runnder

A： User Adnistrator to downlaod SonarQube Runnder plugin ,then you can see it.
See above。

## Q : [TeamCity 2021.1.2 (build 92869)] No enabled compatible agents for this build configuration.Please register a build agent or tweak build configuration requirements.

A :  
先查看 defaut agent 不能用的原因，一般是 Parameters 配置有问题，可以根据错误提示来修复。
如果 fix 完 Parameters 后，还不能使用，只能重新安装一个 Agent。

How to install agent？  
Tab Agents ->Install Build Agents -> Full zip file distribution,  
放入=> /Users/hades/Library/TeamCity/buildAgentFull，go to etc,重命名 buildAgent.dist.properties -> buildAgent.properties  
/Users/hades/Library/TeamCity/bin/runAll.sh stop  
/Users/hades/Library/TeamCity/buildAgent/bin/agent.sh start  
/Users/hades/Library/TeamCity/bin/runAll.sh start

## Q: [TeamCity 2021.1.2 (build 92869)] project code download fail

```
Failed to collect changes, error: List remote refs failed: java.net.SocketTimeoutException: Read timed out, VCS root: "EnableCodeCoverage" {instance id=3, parent internal id=1, parent id=EnableCodeCoverage_EnableCodeCoverage, description: "https://github.com/YingVickyCao/EnableCodeCoverage.git#refs/heads/develop_androidx"}
```

A:  
Version Control Setting -> VCS Root ->Edit ->Edit VCS Root

Authentication method: Anonymous  
=>  
Authentication method: Password / access token  
Username: the user name to access the clone the URL  
Password / access token: the password to clone the URL

## Q : [TeamCity 2021.1.2 (build 92869)]`No files nor directories matching 'app/build/intermediates/classes/debug/'`

```
ERROR: Invalid value for sonar.java.binaries
ERROR: Error during SonarQube Scanner execution
java.lang.IllegalStateException: No files nor directories matching 'app/build/intermediates/classes/debug/'
Process exited with code 1 (Step: SonarQube Analysis (SonarQube Runner))
Step SonarQube Analysis (SonarQube Runner) failed
```

配置：
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
// Android code
-Dsonar.java.binaries=app/build/intermediates/classes/debug/,javaLib/build/classes/java/,androidLib/build/intermediates/classes/debug/
=>
// Androidx code
-Dsonar.java.binaries=app/build/intermediates/javac/debug/classes/,app/build/tmp/kotlin-classes/debug/,javaLib/build/classes/java/main/,javaLib/build/classes/kotlin/main/,androidLib/build/intermediates/javac/debug/classes/,androidLib/build/tmp/kotlin-classes/debug/
```

Dirs:

```
app/build/intermediates/javac/debug/classes/com/github/yingvickycao/enablecodecoverage (Only Java) => Use this
app/build/intermediates/javac/debug/debugUnitTest/com/github/yingvickycao/enablecodecoverage (Only Java)
app/build/intermediates/jacoco_instrumented_classes/debug/out/com/github/yingvickycao/enablecodecoverage (Java + Kotlin，命令行/Teamcity build 没有，AS Click run 有)
app/build/tmp/kotlin-classes/debug/com/github/yingvickycao/enablecodecoverage (Only Kotlin) => Use this

javaLib/build/classes/java/main/com/github/yingvickycao (Only Java) => Use this
javaLib/build/classes/java/test/com/github/yingvickycao (Only Java)
javaLib/build/classes/kotlin/main/com/github/yingvickycao/javalib (Only Kotlin) => Use this
javaLib/build/classes/kotlin/test/com/github/yingvickycao/javalib (Only Kotlin)

androidLib/build/intermediates/javac/debug/classes/com/github/yingvickycao/androidlib (Only Java) => Use this
androidLib/build/intermediates/javac/debugUnitTest/classes/com/github/yingvickycao/androidlib (Only Java)
androidLib/build/intermediates/jacoco_instrumented_classes/debug/out/com/github/yingvickycao/androidlib (Java + Kotlin) =>没有效果
androidLib/build/intermediates/runtime_library_classes_dir/debug/com/github/yingvickycao/androidlib(Java + Kotlin,命令行/Teamcity build 没有，AS Click run 有)
androidLib/build/tmp/kotlin-classes/debug/com/github/yingvickycao/androidlib(Only Kotlin) => Use this
androidLib/build/tmp/kotlin-classes/debugUnitTest/com/github/yingvickycao/androidlib (Only Kotlin)
```

## Q : `java.lang.IllegalStateException: No files nor directories matching 'app/build/intermediates/jacoco_instrumented_classes/debug/'`

A :

```
android studio 4.2.2,need JDK 1.8
androidx code
gradle 6.9
```

androidLib/build/intermediates/runtime_library_classes_dir/debug/com/github/yingvickycao/androidlib(Java + Kotlin,命令行/Teamcity build 没有，AS Click run 有)

# Refs:

- https://www.jianshu.com/p/b30ee02a6b87
- TeamCity 自动触发 Build https://www.cnblogs.com/sparkdev/p/6239431.html
- Notifier Templates Configuration https://confluence.jetbrains.com/display/TCD3/Notifier+Templates+Configuration
- Subscribing to Notifications https://confluence.jetbrains.com/display/TCD10/Subscribing+to+Notifications
- https://docs.sonarqube.org/latest/analysis/scan/sonarscanner-for-gradle/
  https://docs.sonarqube.org/7.9/analysis/analysis-parameters/
- https://docs.sonarqube.org/7.9/analysis/coverage/
- https://docs.sonarqube.org/7.7/analysis/coverage/
- https://docs.sonarqube.org/latest/analysis/external-issues//
- https://docs.sonarqube.org/latest/analysis/languages/java/
- https://docs.sonarqube.org/latest/analysis/languages/kotlin/
- https://plugins.gradle.org/plugin/org.sonarqube
- https://docs.sonarqube.org/display/PLUG/JaCoCo+Plugin
- https://github.com/SonarSource/sonar-scanning-examples/blob/master/doc/jacoco.md
- http://www.cocoachina.com/articles/30404
- Narrowing the Focus https://docs.sonarqube.org/7.9/project-administration/narrowing-the-focus/
- https://www.jianshu.com/p/e384595d0b14
- https://www.jianshu.com/p/778fd35fd494

```

```
