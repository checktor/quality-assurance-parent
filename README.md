# quality-assurance-parent

Parent POM designed to ensure a reliable build by explicitly defining specific versions for Java and Maven as well as its basic plugins. Furthermore, it configures quality assurance components such as [JaCoCo](https://github.com/jacoco/jacoco), [OWASP Dependency-Check](https://github.com/jeremylong/DependencyCheck) and [SpotBugs](https://github.com/spotbugs/spotbugs).

## Requirements

The following build tool versions are required via Maven's enforcer plugin:

* Java 11
* Maven 3.6.3

## Usage

This POM is intended to be used as parent POM of your Maven project:

```
<parent>
    <groupId>io.github.checktor</groupId>
    <artifactId>quality-assurance-parent</artifactId>
    <version>1.1.0</version>
</parent>
```

If you already use a parent declaration, e.g. provided by Spring Boot, consider to move it to `<dependencyManagement>` section using `<type>pom</type>` and `<scope>import</scope>`.

In order to consume a package from GitHub's package registry, you need to define the following additional repository in your project's POM:

```
<repositories>
    <repository>
        <id>github</id>
        <url>https://maven.pkg.github.com/checktor/quality-assurance-parent</url>
    </repository>
</repositories>
```

In this case, we use `github` as the repository ID which should match the ID of your GitHub credentials in local `settings.xml` file:

```
<servers>
    <server>
        <id>github</id>
        <username>your_username</username>
        <password>your_personal_access_token</password>
    </server>
</servers>
```

## OWASP Dependency-Check and SpotBugs filter

This POM assumes the presence of two files inside a [buildSettings](buildSettings/) folder which are used to define filter rules for vulnerability and bug checks, i.e.

* [dependency-check-filter.xml](buildSettings/dependency-check-filter.xml)
    * Suppress reporting of specific OWASP vulnerabilities in your dependencies.
* [spotbugs-filter.xml](buildSettings/spotbugs-filter.xml)
    * Filter specific bug findings.

If you want to suppress the reporting of vulnerability `CVE-2016-1000027`, add the following to your [dependency-check-filter.xml](buildSettings/dependency-check-filter.xml) file.

```
<suppress>
    <cve>CVE-2016-1000027</cve>
</suppress>