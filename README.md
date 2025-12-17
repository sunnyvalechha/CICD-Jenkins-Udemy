







# Maven

- Maven is a JAVA project management tool or build tool.
- It manages source code, test code, libraries, configuration, and dependencies of a project.
- It is based on a POM file (project object model). It is the main configuration file of Maven
- A build tool takes care of everything for building a project.
      * Generate source code.
      * Generate documentation.
      * Complies source code.
      * Install the package code central repo, local repo, or server repo.
  
- Maven can build any number of projects into the desired output, such as '.jar', '.war'
- Maven helps in getting the right .jar file for each project, as there may be different versions of different packages
- Dependencies can be downloaded from 'mvnrepository.com'
- Maven pulls source code from GitHub.

Requirement for build:

* Source code --> Present in workspace
* Compiler -- (Remote repo --> Local repo --> Workspace)
* Dependencies -- (Remote repo --> Local repo --> Workspace)

Maven build life-cycle:

* Generate dependencies
* Compile code
* Unit test
* Package build
* Install (in local repo & artifactory)
* Deploy to servers
* Clean


Some important phases in the default lifecycle include:

* validate: Ensures the project structure and necessary information are correct.
* compile: Compiles the project's source code, typically found in src/main/java.
* test: Executes unit tests.
* package: Bundles the compiled code into a distributable format like a JAR or WAR.
* verify: Runs checks on integration test results to meet quality standards.
* install: Places the project's package in the local Maven repository for use by other local projects.
* deploy: Copies the final package to a remote repository for wider sharing.

Repositories in Maven:

* Local - /home/USER/.m2/repository
* Remote/Private - Nexus/Jfrog
* Central/Public - Public from Maven website

-- Commands:
* mvn install <packagename>
* mvn clean <packagename>
