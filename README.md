# bonfire-oss-parent

Parent POM for the Bonfire Team's open source Java libraries.

```xml
<parent>
    <groupId>today.bonfire.oss</groupId>
    <artifactId>bonfire-oss-parent</artifactId>
    <version>2.1.1</version>
</parent>
```

## What a child inherits

- **Java floor.** Java 21 is enforced as a minimum. Newer JDKs are fine. A child can raise `java.version` but not lower it.
- **Compiler settings.** `--release ${java.version}` with `-Xlint:unchecked` and `-Xlint:deprecation`.
- **`slf4j-api`** at compile scope.
- **Test stack** at test scope: JUnit Jupiter, Mockito (with the inline mock maker loaded as a Java agent), AssertJ, and Logback.
- **Sources and Javadoc jars** attached on every build.
- **OSGi and module manifest.** maven-bundle-plugin writes OSGi headers into every jar. A child that sets the `module.name` property also gets it as `Automatic-Module-Name`; without it, the header is left out.
- **License in the jar.** `LICENSE` and `NOTICE` from the project root are packaged under `META-INF/`. A child that declares its own `<resources>` replaces this list, so it adds `src/main/resources` and the license files itself.
- **Formatting.** Local builds format Java sources with palantir-java-format through Spotless, so there is no separate step to run. CI sets `CI=true` and only checks, so unformatted code fails the build there. `./mvnw spotless:apply` formats without building.
- **Release profiles.** `upload` signs with GPG and stages to Maven Central without publishing. `publish` signs, uploads and waits only until Central has validated the release. Central then publishes it on its own, so it reaches Maven Central a little after the build ends.

## Managed but not inherited

Declare these without a version in the child to use them:

- `org.projectlombok:lombok` (use `provided` scope)
- `org.apache.commons:commons-lang3`
- `com.google.guava:guava`
- `org.jsoup:jsoup`
- `org.jspecify:jspecify`
- Testcontainers modules (`org.testcontainers:*`), through the imported Testcontainers BOM
- `org.apache.maven.plugins:maven-failsafe-plugin` (add it under `<build><plugins>` with the `integration-test` and `verify` goals; it shares Surefire's version and test JVM settings)
- `org.jacoco:jacoco-maven-plugin` (add it under `<build><plugins>`, the agent and report executions come preconfigured; a child that sets its own Surefire or Failsafe `argLine` starts it with `@{argLine}` so the JaCoCo agent is kept)

## Contributing

Follow the [contributing guide](https://github.com/BonfireTrunk/.github/blob/prime/CONTRIBUTING.md). What is specific to this repository:

- **Build.** Needs Java 21 or newer. The Maven Wrapper downloads the pinned Maven version, so no local Maven install is needed:
  ```bash
  ./mvnw verify
  ```
- **Version.** Leave the version in `pom.xml` alone, with one exception: if it is a release version (no `-SNAPSHOT` suffix), yours is the first change since that release, so bump it to the next snapshot. For a bug fix, `2.0.0` becomes `2.0.1-SNAPSHOT`; for a new feature, `2.0.0` becomes `2.1.0-SNAPSHOT`.

## Checking for newer versions

```bash
mvn versions:display-property-updates versions:display-plugin-updates
```
