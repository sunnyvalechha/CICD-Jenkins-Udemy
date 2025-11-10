# Maven

- Maven is a JAVA project management tool or build tool. It manages source code, test code, libraries, configuration and dependencies of a project.
- It is based on POM file (project object model). It is a main configuration file of maven
- A build tool take care of everything for building a project.
      * Generate source code.
      * Generate documentation.
      * Complies source code.
      * Install the package code central repo, local repo or server repo.
- Maven can build any number of projects into desired output such as '.jar', '.war'
- Maven helps in getting the right .jar file for each project as there may be different version of different packages
- Dependencies can be downloaded from 'mvnrepository.com'
- Maven pulls source code from Github.

-- Requirement for build 
* Source code --> Present in workspace
* Compiler -- (Remote repo --> Local repo --> Workspace)
* Dependencies -- (Remote repo --> Local repo --> Workspace)

-- Maven build life-cycle
* Generate dependencies
* Compile code
* Unit test
* Package build
* Install (in local repo & artifactory)
* Deploy to servers
* Clean

-- Commands:
* mvn install <packagename>
* mvn clean <packagename>
