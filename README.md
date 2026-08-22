# Spring Boot Java Template

[Japanese](README-ja.md)

This repository is a starting point for Spring Boot applications. It includes
equivalent Maven and Gradle configurations so that a project can choose one
build system after it is created from the template.

The two configurations describe the same application. They are not intended to
remain as parallel sources of build configuration in a derived project.

## Requirements

- JDK 25
- A POSIX-compatible shell for `mvnw` or `gradlew` on macOS and Linux
- PowerShell or Command Prompt for `mvnw.cmd` or `gradlew.bat` on Windows

The wrappers download the required Maven or Gradle distribution, so a global
installation of either build tool is not required.

## Create a project from this template

1. Select **Use this template** on GitHub and create a repository.
2. Clone the new repository.
3. Replace the sample coordinates, package, and application name.
4. Choose Maven or Gradle and remove the files for the other build system.
5. Run the build for the selected system.

At minimum, update these sample values:

- `dev.sobue.demo` in the Java package and build configuration
- `demo` in the artifact name, root project name, and `spring.application.name`
- `0.0.1-SNAPSHOT` if the project uses a different initial version

## Choose a build system

### Maven

Keep these files and directories:

- `pom.xml`
- `.mvn/`
- `mvnw`
- `mvnw.cmd`
- `.github/workflows/maven.yml`

Remove these Gradle-specific files and directories:

- `build.gradle`
- `settings.gradle`
- `gradle/`
- `gradlew`
- `gradlew.bat`
- `.github/workflows/gradle.yml`

Build and run the application:

```sh
./mvnw --batch-mode verify
./mvnw spring-boot:run
```

### Gradle

Keep these files and directories:

- `build.gradle`
- `settings.gradle`
- `gradle/`
- `gradlew`
- `gradlew.bat`
- `.github/workflows/gradle.yml`

Remove these Maven-specific files and directories:

- `pom.xml`
- `.mvn/`
- `mvnw`
- `mvnw.cmd`
- `.github/workflows/maven.yml`

Build and run the application:

```sh
./gradlew build
./gradlew bootRun
```

## Native image

GraalVM is required to build a native executable.

With Maven:

```sh
./mvnw --batch-mode -Pnative native:compile -DskipTests
```

With Gradle:

```sh
./gradlew nativeCompile
```

## Continuous integration

The template contains matching Maven and Gradle workflows. Each workflow:

- builds and tests the application on JDK 25 and JDK 26;
- builds a native executable with GraalVM JDK 25; and
- runs a SonarQube scan only when the `SONAR_TOKEN` repository secret is set.

After choosing a build system, keep only its workflow. This prevents duplicate
builds and keeps the selected build definition authoritative.

## Project layout

```text
src/main/java/       Application source code
src/main/resources/  Application configuration and resources
src/test/java/       Automated tests
.github/workflows/   Maven and Gradle CI alternatives
```
