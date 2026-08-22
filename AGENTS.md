# AGENTS.md

## Repository purpose

This repository is a template for a Spring Boot Java application. The template
contains equivalent Maven and Gradle builds so that a derived repository can
choose one of them. Do not introduce behavior in only one build definition
unless the task explicitly concerns that build system.

In a derived repository, Maven and Gradle are alternatives rather than two
permanent build paths. Once a build system has been selected, remove the other
build definition, wrapper, and CI workflow.

## Source of truth

Before changing dependencies, plugins, Java compatibility, test behavior,
packaging, or native-image settings, inspect both `pom.xml` and `build.gradle`.
Keep their application behavior aligned while this repository remains a
template.

Keep these values consistent across the repository:

- Maven `groupId` and Gradle `group`
- Maven `artifactId`, Gradle root project name, and the application name
- project version
- Java compatibility
- Spring Boot, JaCoCo, and GraalVM build-tool versions where applicable
- production and test dependencies

## Build and verification

Use the checked-in wrappers. Do not require a globally installed Maven or
Gradle distribution.

Java version differences can change a build result. Do not treat a result from
one JDK as sufficient. Verify the regular Maven and Gradle builds with every JDK
version supported by the CI matrix.

On macOS, select the JDK with `/usr/libexec/java_home`. For example:

```sh
export JAVA_HOME="$(/usr/libexec/java_home -v 25)"
./mvnw --batch-mode verify
./gradlew build
```

Repeat the builds with JDK 26 when it is available locally. If a supported JDK
is unavailable, report which verification could not be run.

Use these primary verification commands:

```sh
./mvnw --batch-mode verify
./gradlew build
```

Run native-image builds when changing native configuration, reflection or
resource metadata, packaging, or GraalVM-related dependencies:

```sh
./mvnw --batch-mode -Pnative native:compile -DskipTests
./gradlew nativeCompile
```

## Continuous integration

Keep `.github/workflows/maven.yml` and `.github/workflows/gradle.yml` equivalent
while both build systems exist. Preserve the JDK 25 and JDK 26 build matrix and
the GraalVM JDK 25 native build unless a task changes the supported versions.

SonarQube scans are optional. The scan job must run only when the
`check-sonarqube-token` job reports that `SONAR_TOKEN` is configured. Match job
IDs exactly in `needs`, output references, and conditions.

Pin GitHub Actions to full commit SHAs. When updating an action, retain a comment
that identifies the corresponding release version.

## Java documentation and comments

Add Javadoc to every production `class`, `interface`, `enum`, and `record`.
Add Javadoc to constructors, methods, and fields unless the element is a simple
accessor or an override whose contract is already complete and no additional
information would be provided. Javadoc is mandatory for public and protected
constructors, methods, and fields.

Document responsibilities, contracts, preconditions, return values, exceptions,
lifecycle, and side effects as applicable. Include `@param`, `@return`, and
`@throws` when required by the contract. Write comments and Javadoc in English.

In test code, add Javadoc to test classes but not to test methods. Use
`@DisplayName` to state the behavior and verification point of each test.

Use ordinary comments only to explain why an alternative was rejected when that
reason cannot be expressed through naming, types, tests, or structure. Do not
write comments that narrate the code.

## Documentation

Keep `README.md` and `README-ja.md` structurally equivalent. Write
`README.md` and this file in English, and write `README-ja.md` in Japanese.
Update both READMEs when commands, supported JDKs, build-system selection, CI,
or the project layout changes.

Record durable development notes under `Development Notes/` when a change needs
context that does not belong in the README or source documentation.

## Git

Do not commit generated output from `build/`, `target/`, IDE metadata, or
`.DS_Store` files.

When an agent contributes to a commit requested by the user, append this trailer
using the actual agent name and model slug:

```text
Assisted-by: <agent>:<model-slug>
```

Do not use `Co-Authored-By` for the agent. Use `/opt/homebrew/bin/gh` rather than
a GitHub MCP integration for GitHub operations.
