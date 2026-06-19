### CI/CD Pipeline with Jenkins, Maven, SonarQube, and Nexus
Project Overview

This project demonstrates the implementation of a Continuous Integration and Continuous Delivery (CI/CD) pipeline using Jenkins, Maven, SonarQube, and Nexus Repository Manager. The pipeline automates application build, code quality analysis, artifact management, and deployment preparation.
   
### Prerequisites
- JDK 1.8 or later
- Maven 3 or later
- MySQL 5.6 or later

### Technologies 
- Git
- GitHub
- Jenkins
- Maven
- SonarQube
- Nexus Repository Manager
- Java (JDK 8/11/17/21)
- Linux (Ubuntu/Amazon Linux)
- Bash Scripting
### Components 
### Jenkins

Jenkins serves as the CI server and automates the build pipeline.

Responsibilities:

- Pull source code from GitHub
- Execute Maven builds
- Trigger SonarQube scans
- Upload artifacts to Nexus
- Generate build reports

### Maven

Maven is used for dependency management and application builds.

Common Commands:

mvn clean package
mvn deploy

## SonarQube

SonarQube performs static code analysis and quality checks.

Stores:

- Code analysis results
- Project history
- Quality Gates

## Database:

- PostgreSQL
- Nexus Repository Manager

Nexus acts as the centralized artifact repository.

Repositories:

- maven-releases-
- maven-snapshots
- maven-central (proxy)
- maven-public (group)=vpro-maven-group

Stores:

- WAR files
- Build artifacts
## GitHub

GitHub hosts the source code and triggers Jenkins builds through webhooks.

- Jenkins Configuration
-  JDK Configuration: JAVA_HOME: /usr/lib/jvm/java-21-openjdk-amd64
-  Maven Configuration: Install Automatically: Enabled
  
## Jenkins Plugins Used

To integrate Jenkins with external tools and automate the CI/CD workflow, the following plugins were installed:

### GitHub Integration Plugin
- Connects Jenkins with GitHub repositories
- Supports webhooks for automatic build triggering
- Enables source code checkout from GitHub
### Maven Integration Plugin
- Integrates Apache Maven with Jenkins
- Manages project dependencies
- Builds and packages Java applications
### SonarQube Scanner Plugin
- Integrates SonarQube with Jenkins
- Performs static code analysis
- Enforces Quality Gates and code quality standards
### Nexus Artifact Uploader Plugin
- Publishes build artifacts to Nexus Repository Manager
- Supports JAR and WAR artifact uploads
- Enables centralized artifact management
### Slack Notification Plugin
- Sends build and deployment notifications to Slack
- Provides real-time feedback on pipeline status
- Alerts teams about build failures and successes
### Build Timestamp Plugin
- Adds timestamps to Jenkins builds
- Improves build traceability
- Helps generate unique artifact names
  
### Nexus Dependency Management

- Nexus is configured as a proxy for Maven Central.

- Jenkins → Nexus → Maven Central

- During a build, Maven dependencies are downloaded through Nexus. If a dependency is not available locally, Nexus retrieves it from Maven Central and caches it for future builds.

#### Maven Central Repository:

https://repo1.maven.org/maven2/

Benefits:

- Faster builds
- Dependency caching
- Reduced internet traffic
- Centralized dependency management


