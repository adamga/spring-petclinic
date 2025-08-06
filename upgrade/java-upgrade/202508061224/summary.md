
# Upgrade Java Project

## 🖥️ Project Information
- **Project path**: c:\Repos\spring-petclinic
- **Java version**: 21
- **Build tool type**: Maven Wrapper
- **Build tool path**: c:\Repos\spring-petclinic

## 🎯 Goals

- Upgrade Java to 21

## 🔀 Changes

### Test Changes
|     | Total | Passed | Failed | Skipped | Errors |
|-----|-------|--------|--------|---------|--------|
| Before | 45 | 43 | 0 | 2 | 0 |
| After | 45 | 43 | 0 | 2 | 0 |
### Dependency Changes


#### Upgraded Dependencies
| Dependency | Original Version | Current Version | Module |
|------------|------------------|-----------------|--------|
| org.springframework.boot:spring-boot-starter-actuator | 3.2.1 | 3.2.12 | spring-petclinic |
| org.springframework.boot:spring-boot-starter-cache | 3.2.1 | 3.2.12 | spring-petclinic |
| org.springframework.boot:spring-boot-starter-data-jpa | 3.2.1 | 3.2.12 | spring-petclinic |
| org.springframework.boot:spring-boot-starter-web | 3.2.1 | 3.2.12 | spring-petclinic |
| org.springframework.boot:spring-boot-starter-validation | 3.2.1 | 3.2.12 | spring-petclinic |
| org.springframework.boot:spring-boot-starter-thymeleaf | 3.2.1 | 3.2.12 | spring-petclinic |
| org.springframework.boot:spring-boot-starter-test | 3.2.1 | 3.2.12 | spring-petclinic |
| com.mysql:mysql-connector-j | 8.1.0 | 8.3.0 | spring-petclinic |
| org.postgresql:postgresql | 42.6.0 | 42.6.2 | spring-petclinic |
| org.springframework.boot:spring-boot-devtools | 3.2.1 | 3.2.12 | spring-petclinic |
| org.springframework.boot:spring-boot-testcontainers | 3.2.1 | 3.2.12 | spring-petclinic |
| org.springframework.boot:spring-boot-docker-compose | 3.2.1 | 3.2.12 | spring-petclinic |
| org.testcontainers:junit-jupiter | 1.19.3 | 1.19.8 | spring-petclinic |
| org.testcontainers:mysql | 1.19.3 | 1.19.8 | spring-petclinic |
| jakarta.xml.bind:jakarta.xml.bind-api | 4.0.1 | 4.0.2 | spring-petclinic |
| Java | 17 | 21 | Root Module |

### Code commits
1 file changed, 7 insertions(+), 4 deletions(-)

- 318bc3f -- Upgrade project to Java 21 using openrewrite.

- d92c2b0 -- Upgrade Spring Boot to version 3.2.12 for security fixes

- bd5564d -- Override Tomcat version to 10.1.42 for CVE fixes

### Potential Issues
