# pub-pkg

## Agent Android lib publish

Generate a `Personal Token (classic)` on GitHub profile -> settings -> Developer Settings -> Personal Access -> Token (classic). Apply all repo options and write:package. Generate and copy the token.

### Publishing with `mvn` command

* Configure local repository maven with `settings.xml` file
  * create a file `settings.xml` inside directory `m2`

```xml
<settings xmlns="http://maven.apache.org/SETTINGS/1.0.0"
      xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
      xsi:schemaLocation="http://maven.apache.org/SETTINGS/1.0.0
                          https://maven.apache.org/xsd/settings-1.0.0.xsd">
  <servers>
    <server>
      <id>github</id>
      <username>$GITHUB_USERNAME</username>
      <password>$PERSONAL_TOKEN</password>
    </server>
  </servers>
</settings>
```

> Replace $GIT_USERNAME with your userName on github. Replace $PERSONAL_TOKEN with copied token from access.

* Deploy version to Maven repo

```bash
mvn deploy:deploy-file \
    -DrepositoryId=github \
    -Dfile=JARFILE \
    -DgroupId=$GROUP_ID \
    -DartifactId=$ARTIFACT_ID \
    -Dversion=$VERSION \
    -Dpackaging=$JAR_OR_AAR \
    -DgeneratePom=true \
    -DcreateChecksum=true \
    -Durl=https://maven.pkg.github.com/contabilone/pub-pkg
```

example:

```
mvn deploy:deploy-file \
    -DrepositoryId=github \
    -Dfile=lib-release.aar \
    -DgroupId=ai.omnitax \
    -DartifactId=agent-android \
    -Dversion=1.0.0 \
    -Dpackaging=aar \
    -DgeneratePom=true \
    -DcreateChecksum=true \
    -Durl=https://maven.pkg.github.com/contabilone/priv-pkg
```

### Deploy with Gradle

Project is configured for publish. All it needs to do is configure a `gradle.propeerties` file with GitHub credentials.

* Create `gradle.properties` file in global gradle diirectory, at HOME

```properties
GITHUB_ACTOR=$user_name
GITHUB_TOKEN=$personal_token
```

> Use the keys of properties file as documented, the library build.gradle is configured with these names. Replace variable with your informations. remember, generate de Personal Token and copy it. After save the Personal token, it is impossible to show it again.

* On Intellij IDEA, open the Gradle menu and click on `publishing -> publish`
* On terminal, run the command on the root project directory

  ```bash
  ./gradlew :lib:publish
  ```


