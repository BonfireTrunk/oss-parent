# bonfire-oss-parent

Parent POM for the Bonfire Team's open source Java libraries.

```xml
<parent>
    <groupId>today.bonfire.oss</groupId>
    <artifactId>bonfire-oss-parent</artifactId>
    <version>2.0.0</version>
</parent>
```

## What a child inherits

- **Java floor.** Java 21 is enforced as a minimum. Newer JDKs are fine. A child can raise `java.version` but not lower it.
- **Compiler settings.** `--release ${java.version}` with `-Xlint:unchecked` and `-Xlint:deprecation`.
- **`slf4j-api`** at compile scope.
- **Test stack** at test scope: JUnit Jupiter, Mockito (with the inline mock maker loaded as a Java agent), AssertJ, and Logback.
- **Sources and Javadoc jars** attached on every build.
- **Formatting.** Local builds format Java sources with palantir-java-format through Spotless, so there is no separate step to run. CI sets `CI=true` and only checks, so unformatted code fails the build there. `./mvnw spotless:apply` formats without building.
- **Release profiles.** `upload` signs with GPG and stages to Maven Central without publishing. `publish` signs, auto-publishes and waits until the release is live on Central.

## Managed but not inherited

Declare these without a version in the child to use them:

- `org.projectlombok:lombok` (use `provided` scope)
- `org.apache.commons:commons-lang3`
- `com.google.guava:guava`
- `org.jsoup:jsoup`
- `org.jacoco:jacoco-maven-plugin` (add it under `<build><plugins>`, the agent and report executions come preconfigured)

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
