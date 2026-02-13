# SCOS BOM (SawCunha Open System)

[![Build & Test](https://github.com/sawcunha/sawcunha-open-system-bom/actions/workflows/build.yml/badge.svg)](https://github.com/sawcunha/sawcunha-open-system-bom/actions/workflows/build.yml)
[![Maven Central](https://img.shields.io/maven-central/v/io.github.sawcunha/scos-bom.svg?label=Maven%20Central)](https://central.sonatype.com/artifact/io.github.sawcunha/scos-bom)
[![License](https://img.shields.io/badge/License-Apache%202.0-blue.svg)](LICENSE)
[![Java 25+](https://img.shields.io/badge/Java-25%2B-orange.svg)](https://www.java.com)
[![Maven 3.9.12+](https://img.shields.io/badge/Maven-3.9.12%2B-blue.svg)](https://maven.apache.org/)

Official Bill of Materials (BOM) that standardizes and aligns dependency versions across all SCOS projects.

---

## 📦 Overview

The **SCOS BOM** provides centralized dependency management for all projects within the SawCunha Open System ecosystem.

By importing this BOM, applications and libraries can:

* Use consistent, tested dependency versions across all modules
* Reduce version conflicts (dependency hell)
* Simplify dependency upgrades and maintenance
* Ensure compatibility across the entire SCOS ecosystem
* Align with SCOS platform standards and best practices
* Benefit from enterprise-grade dependency curation

This project follows semantic versioning and is designed for production use with Maven and Gradle builds.

---

## 📋 Supported Technology Stack

### Core Platforms
- **Java**: 25 or higher (LTS)
- **Build Tools**: Maven 3.9.12+
- **License**: Apache License 2.0

### Spring Ecosystem (Current Versions)

| Component            | Version  | Notes                                |
|----------------------|----------|--------------------------------------|
| **Spring Boot**      | 4.0.2    | Latest stable release, Java 25 ready |
| **Spring Framework** | 7.0.3    | Jakarta EE, Virtual Threads ready    |
| **Spring Cloud**     | 2025.1.1 | Latest cloud-native features         |
| **Spring Data**      | 2025.1.2 | JPA, MongoDB, Redis support          |
| **Spring Kafka**     | 4.0.2    | Event streaming integration          |

### Database & Persistence

| Component                  | Version     | Purpose                         |
|----------------------------|-------------|---------------------------------|
| **PostgreSQL JDBC**        | 42.7.9      | PostgreSQL relational database  |
| **MySQL Connector**        | 9.5.0       | MySQL relational database       |
| **Redis (Jedis)**          | 7.2.0       | In-memory data cache            |
| **Hibernate ORM**          | 7.2.0.Final | Object-relational mapping       |
| **JPA (Jakarta)**          | 6.x         | Standard persistence API        |
| **Liquibase**              | 5.0.1       | Database migration & versioning |
| **QueryDSL**               | 7.1         | Type-safe query builder         |
| **ShedLock**               | 7.3.0       | Distributed task scheduling     |
| **Spatial** (JTS/GeoTools) | 1.20.0+     | Geospatial support              |

### Serialization & Data Formats

| Component     | Version  | Purpose                              |
|---------------|----------|--------------------------------------|
| **Jackson**   | 3.0.4    |  JSON/XML binding (with all modules) |
| **GSON**      | 2.13.2   | JSON serialization alternative       |
| **SnakeYAML** | 2.5      | YAML configuration parsing           |
| **org.json**  | 20250517 | JSON processing utility              |

### Testing & Quality Assurance

| Component          | Version        | Purpose                           |
|--------------------|----------------|-----------------------------------|
| **JUnit**          | 6.0.2          | Unit testing framework            |
| **Mockito**        | 5.21.0         | Mock object framework             |
| **TestContainers** | 2.0.3          | Container-based integration tests |
| **WireMock**       | 3.13.2 / 4.0.8 | HTTP API mocking                  |
| **REST Assured**   | 6.0.0          | REST API testing                  |
| **JaCoCo**         | 0.8.13         | Code coverage measurement         |

### Utilities & Libraries

| Component               | Version    | Purpose                    |
|-------------------------|------------|----------------------------|
| **Project Lombok**      | 1.18.42    | Boilerplate code reduction |
| **MapStruct**           | 1.6.3      | Bean mapping processor     |
| **Apache Commons IO**   | 2.21.0     | I/O utilities              |
| **Apache Commons Lang** | 3.20.0     | Language utilities         |
| **Google Guava**        | 33.5.0-jre | Collections & utilities    |
| **Reflections**         | 0.10.2     | Reflection utilities       |
| **Caffeine**            | 3.2.3      | High-performance cache     |

### Logging & Monitoring

| Component            | Version      | Purpose                     |
|----------------------|--------------|-----------------------------|
| **SLF4J**            | (via Spring) | Logging facade              |
| **Logback**          | (via Spring) | Logging implementation      |
| **GELF Encoder**     | 6.1.2        | Graylog Extended Log Format |
| **Logstash Encoder** | 9.0          | ELK Stack integration       |

### Security & Cryptography

| Component         | Version  | Purpose               |
|-------------------|----------|-----------------------|
| **Bouncy Castle** | 1.83     | Cryptography provider |
| **Jasypt**        | 4.0.3    | Property encryption   |

### AOP & HTTP

| Component     | Version  | Purpose                     |
|---------------|----------|-----------------------------|
| **AspectJ**   | 1.9.25.1 | Aspect-oriented programming |
| **OpenFeign** | 13.6     | Declarative HTTP client     |

---

## 🚀 Usage

### Maven

Import the BOM in your `dependencyManagement` section of `pom.xml`:

```xml
<dependencyManagement>
  <dependencies>
    <dependency>
      <groupId>io.github.sawcunha</groupId>
      <artifactId>scos-bom</artifactId>
      <version>1.0.0</version>
      <type>pom</type>
      <scope>import</scope>
    </dependency>
  </dependencies>
</dependencyManagement>
```

Then declare dependencies **without versions** (they will be managed by BOM):

```xml
<dependencies>
  <!-- Spring Boot Starter Web -->
  <dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-web</artifactId>
  </dependency>

  <!-- PostgreSQL Driver -->
  <dependency>
    <groupId>org.postgresql</groupId>
    <artifactId>postgresql</artifactId>
  </dependency>

  <!-- Jackson for JSON -->
  <dependency>
    <groupId>com.fasterxml.jackson.core</groupId>
    <artifactId>jackson-databind</artifactId>
  </dependency>

  <!-- Testing -->
  <dependency>
    <groupId>org.junit.jupiter</groupId>
    <artifactId>junit-jupiter</artifactId>
    <scope>test</scope>
  </dependency>
</dependencies>
```

---

### Gradle (Kotlin DSL)

```kotlin
dependencyManagement {
    imports {
        mavenBom("io.github.sawcunha:scos-bom:1.0.0")
    }
}

dependencies {
    implementation("org.springframework.boot:spring-boot-starter-web")
    implementation("org.postgresql:postgresql")
    implementation("com.fasterxml.jackson.core:jackson-databind")
    
    testImplementation("org.junit.jupiter:junit-jupiter")
}
```

---

### Gradle (Groovy DSL)

```groovy
dependencyManagement {
    imports {
        mavenBom 'io.github.sawcunha:scos-bom:1.0.0'
    }
}

dependencies {
    implementation 'org.springframework.boot:spring-boot-starter-web'
    implementation 'org.postgresql:postgresql'
    implementation 'com.fasterxml.jackson.core:jackson-databind'
    
    testImplementation 'org.junit.jupiter:junit-jupiter'
}
```

---

## 🧩 What This BOM Manages

This BOM manages dependencies in these categories:

### Spring Ecosystem
- Spring Boot (core, starters)
- Spring Framework
- Spring Cloud (microservices patterns)
- Spring Data (database access)
- Spring Kafka (event streaming)

### Databases & Persistence
- JDBC drivers (PostgreSQL, MySQL)
- Redis client
- Hibernate ORM
- JPA/Jakarta EE
- Migration tools (Liquibase)
- Query builders (QueryDSL)
- Spatial/GIS libraries

### Testing & Quality
- JUnit 5 & 6
- Mocking frameworks
- Integration test containers
- HTTP mocking (WireMock)
- Code coverage (JaCoCo)

### Serialization
- Jackson (primary, all modules)
- GSON alternative
- YAML/JSON parsers

### Development Tools
- Lombok (boilerplate reduction)
- MapStruct (bean mapping)
- Apache Commons libraries
- Google Guava

### Logging & Monitoring
- SLF4J/Logback
- ELK Stack integrations
- Centralized logging (GELF)

### Security & Cryptography
- Bouncy Castle
- Jasypt encryption

---

## 📊 Compatibility Matrix

| SCOS BOM Version   | Java  | Maven   | Spring Boot   | Spring Framework   | Spring Cloud   |
|--------------------|-------|---------|---------------|--------------------|----------------|
| **1.0.x**          | 25+   | 3.9.12+ | 4.0.2         | 7.0.3              | 2025.1.1       |

**Notes:**
- Each BOM version represents a tested and compatible set of dependencies
- Mixing versions from different BOM releases is discouraged
- Java 25 is LTS (Long Term Support) from Oracle
- All versions are production-ready

---

## 🔢 Versioning

This project follows **Semantic Versioning (SemVer)**:

```
1.2.3
│ │ │
│ │ └─ PATCH: Dependency updates, security fixes (1.2.0 → 1.2.1)
│ └──── MINOR: New dependencies, backward-compatible (1.2.0 → 1.3.0)
└────── MAJOR: Breaking changes in managed dependencies (1.2.0 → 2.0.0)
```

### Release Process

1. **Development**: Changes merged to `develop` branch
2. **Tag Creation**: Maintainers create git tag (e.g., `v1.0.0`)
3. **CI/CD Triggered**: GitHub Actions automatically:
   - Builds the package
   - Signs artifacts with GPG
   - Publishes to Maven Central
   - Creates GitHub Release with notes
4. **Available**: Published within minutes to Maven Central

---

## 🏗️ Project Standards

### Build Requirements
- **Java**: 25 or higher
- **Maven**: 3.9.12 or higher
- **Build Type**: Maven (POM-only, no sources)

### Code Quality
- Code coverage via JaCoCo
- Static analysis via Checkstyle
- Dependency vulnerability checks (OWASP)
- Automatic GPG code signing for releases

### CI/CD Pipeline
- **Build Workflow**: Tests on every push/PR
- **Security Checks**: Dependency and code quality analysis
- **Release Workflow**: Automated Maven Central publishing on git tags

---

## 🤝 Contributing

Contributions are welcome! Whether it's:

- Adding new dependencies
- Updating versions
- Improving documentation
- Reporting issues
- Suggesting enhancements

**Before submitting a change:**

1. **Open an issue** describing your proposal
2. **Discuss compatibility** impact and justification
3. **Update documentation** if adding/changing dependencies
4. **Verify builds pass** locally: `mvn clean verify`
5. **Follow commit conventions**: See [CONTRIBUTING.md](CONTRIBUTING.md)

### Development Setup

```bash
# Clone repository
git clone https://github.com/SawCunhaOS/sawcunha-open-system-bom.git
cd sawcunha-open-system-bom

# Build
mvn clean install

# Run checks
mvn -Panalyze clean verify

# View contribution guidelines
cat CONTRIBUTING.md
```

For detailed contribution guidelines, see [CONTRIBUTING.md](CONTRIBUTING.md).

---

## 📜 License

This project is licensed under the **Apache License 2.0**.

You are free to:
- ✅ Use commercially
- ✅ Modify the code
- ✅ Distribute derivatives
- ✅ Use privately

Provided that you:
- 📝 Include a copy of the license
- 📝 State significant changes made
- 📝 Include a NOTICE file

See the [LICENSE](LICENSE) file for complete details.

---

## 🧭 About SawCunha Open System (SCOS)

SawCunha Open System is an open ecosystem of modular, production-grade software components designed for:

- **Reliability**: Enterprise-grade stability and compatibility
- **Interoperability**: Seamless integration between modules
- **Maintainability**: Long-term support and upgrade paths
- **Innovation**: Modern frameworks and best practices

The SCOS BOM serves as the foundational dependency standard for all SCOS projects.

---

## ⭐ Support

If this project is useful to you, please consider:

- ⭐ Starring the repository on GitHub
- 🐛 Reporting bugs and issues
- 💡 Suggesting improvements
- 📢 Sharing with your team

Your support helps maintain and improve this project!

---

## 📫 Contact & Resources

### Documentation
- [CONTRIBUTING.md](CONTRIBUTING.md) - Contribution guidelines
- [CHANGELOG.md](CHANGELOG.md) - Version history and changes
- [Maven Central](https://central.sonatype.com/artifact/io.github.sawcunha/scos-bom) - Repository info

### Community
- [GitHub Issues](https://github.com/sawcunha/sawcunha-open-system-bom/issues) - Report bugs, request features
- [GitHub Discussions](https://github.com/sawcunha/sawcunha-open-system-bom/discussions) - Ask questions
- [GitHub Releases](https://github.com/sawcunha/sawcunha-open-system-bom/releases) - Version history

### Maintained by
The **SawCunha Open System Community**

For questions or support, open an issue on GitHub or join discussions.

---

## 🔗 Quick Links

- **Maven Central**: https://central.sonatype.com/artifact/io.github.sawcunha/scos-bom
- **GitHub Repository**: https://github.com/sawcunha/sawcunha-open-system-bom
- **GitHub Organization**: https://github.com/sawcunha
- **License**: Apache 2.0

---

**Last Updated**: February 2024  
**Current Stable Version**: 1.0.0
